<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Mobile Gesture Particles</title>
    <style>
        body { margin: 0; overflow: hidden; background-color: #000; font-family: sans-serif; position: fixed; width: 100%; height: 100%; }
        canvas { display: block; }

        /* Overlay Start - Penting untuk Mobile */
        #overlay {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.9);
            display: flex; flex-direction: column; justify-content: center; align-items: center;
            z-index: 1000; color: white; text-align: center; padding: 20px;
        }
        
        button#start-btn {
            padding: 15px 40px; font-size: 18px; font-weight: bold;
            background: #00ccff; border: none; border-radius: 30px;
            color: white; cursor: pointer; margin-top: 20px;
        }

        /* Video Preview Kecil */
        #video-container {
            position: absolute; bottom: 10px; left: 10px;
            width: 100px; height: 75px; border-radius: 8px;
            overflow: hidden; border: 1px solid rgba(255, 255, 255, 0.3);
            transform: scaleX(-1); z-index: 5;
        }
        video { width: 100%; height: 100%; object-fit: cover; }

        /* Fullscreen Button */
        #fs-btn {
            position: absolute; bottom: 10px; right: 10px;
            width: 44px; height: 44px; background: rgba(255,255,255,0.2);
            border: none; border-radius: 50%; color: white;
            font-size: 20px; z-index: 5; cursor: pointer;
        }

        /* Sembunyikan lil-gui bawaan jika di mobile agar tidak sempit */
        @media screen and (max-width: 600px) {
            .lil-gui { width: 160px !important; font-size: 11px !important; }
        }
    </style>
</head>
<body>

<div id="overlay">
    <h2>3D GESTURE PARTICLES</h2>
    <p>Gunakan gerakan mencubit/membuka jari ke arah kamera untuk mengontrol partikel.</p>
    <button id="start-btn">MULAI SISTEM</button>
</div>

<div id="video-container">
    <video id="webcam" autoplay playsinline muted></video>
</div>

<button id="fs-btn" onclick="toggleFS()">⛶</button>

<script type="importmap">
    {
        "imports": {
            "three": "https://unpkg.com/three@0.160.0/build/three.module.js",
            "lil-gui": "https://cdn.jsdelivr.net/npm/lil-gui@0.19/+esm"
        }
    }
</script>

<script type="module">
    import * as THREE from 'three';
    import { GUI } from 'lil-gui';
    import { HandLandmarker, FilesetResolver } from "https://cdn.jsdelivr.net/npm/@mediapipe/tasks-vision@0.10.0";

    let scene, camera, renderer, particles, handLandmarker;
    let video = document.getElementById('webcam');
    const particleCount = 7000; // Dikurangi sedikit agar lancar di Android
    const targets = { sphere: [], box: [], torus: [], cloud: [] };
    
    const state = {
        pattern: 'sphere',
        color: '#00ffff',
        size: 0.04,
        expansion: 1.0,
        rotationSpeed: 0.005
    };

    // Inisialisasi saat tombol diklik
    document.getElementById('start-btn').addEventListener('click', async () => {
        document.getElementById('overlay').style.display = 'none';
        await init();
    });

    async function init() {
        // 1. Scene Setup
        scene = new THREE.Scene();
        camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
        camera.position.z = 5;

        renderer = new THREE.WebGLRenderer({ antialias: false }); // Antialias off untuk performa mobile
        renderer.setSize(window.innerWidth, window.innerHeight);
        renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2)); // Batasi pixel ratio agar GPU tidak berat
        document.body.appendChild(renderer.domElement);

        // 2. Patterns
        generatePatterns();

        // 3. Particles
        const geo = new THREE.BufferGeometry();
        const pos = new Float32Array(particleCount * 3);
        for(let i=0; i<particleCount*3; i++) pos[i] = (Math.random()-0.5)*10;
        geo.setAttribute('position', new THREE.BufferAttribute(pos, 3));

        const mat = new THREE.PointsMaterial({
            size: state.size,
            color: state.color,
            transparent: true,
            blending: THREE.AdditiveBlending,
            depthWrite: false,
            map: createCircleTexture()
        });

        particles = new THREE.Points(geo, mat);
        scene.add(particles);

        // 4. AI & Camera
        await setupAI();
        setupUI();
        animate();
    }

    function createCircleTexture() {
        const canvas = document.createElement('canvas');
        canvas.width = 64; canvas.height = 64;
        const ctx = canvas.getContext('2d');
        const grad = ctx.createRadialGradient(32, 32, 0, 32, 32, 32);
        grad.addColorStop(0, 'rgba(255,255,255,1)');
        grad.addColorStop(1, 'rgba(255,255,255,0)');
        ctx.fillStyle = grad;
        ctx.fillRect(0, 0, 64, 64);
        return new THREE.CanvasTexture(canvas);
    }

    function generatePatterns() {
        for (let i = 0; i < particleCount; i++) {
            // Sphere
            const p = Math.acos(-1 + (2 * i) / particleCount);
            const t = Math.sqrt(particleCount * Math.PI) * p;
            targets.sphere.push(2*Math.cos(t)*Math.sin(p), 2*Math.sin(t)*Math.sin(p), 2*Math.cos(p));
            
            // Box
            targets.box.push((Math.random()-0.5)*4, (Math.random()-0.5)*4, (Math.random()-0.5)*4);
            
            // Torus
            const u = Math.random()*Math.PI*2; const v = Math.random()*Math.PI*2;
            targets.torus.push((2+0.7*Math.cos(v))*Math.cos(u), (2+0.7*Math.cos(v))*Math.sin(u), 0.7*Math.sin(v));

            // Cloud (Random)
            targets.cloud.push((Math.random()-0.5)*10, (Math.random()-0.5)*10, (Math.random()-0.5)*10);
        }
    }

    async function setupAI() {
        const vision = await FilesetResolver.forVisionTasks("https://cdn.jsdelivr.net/npm/@mediapipe/tasks-vision@0.10.0/wasm");
        handLandmarker = await HandLandmarker.createFromOptions(vision, {
            baseOptions: {
                modelAssetPath: "https://storage.googleapis.com/mediapipe-models/hand_landmarker/hand_landmarker/float16/1/hand_landmarker.task",
                delegate: "GPU"
            },
            runningMode: "VIDEO", numHands: 1
        });

        const constraints = { 
            video: { 
                facingMode: "user", 
                width: { ideal: 640 }, 
                height: { ideal: 480 } 
            } 
        };
        const stream = await navigator.mediaDevices.getUserMedia(constraints);
        video.srcObject = stream;
    }

    function setupUI() {
        const gui = new GUI();
        gui.add(state, 'pattern', Object.keys(targets)).name('Pola Visual');
        gui.addColor(state, 'color').name('Warna').onChange(v => particles.material.color.set(v));
        gui.add(state, 'size', 0.01, 0.15).name('Ukuran Titik').onChange(v => particles.material.size = v);
        gui.close(); // Tutup GUI di mobile secara default
    }

    function animate() {
        requestAnimationFrame(animate);

        // Morphing Logic
        const posAttr = particles.geometry.attributes.position;
        const targetArr = targets[state.pattern];
        for (let i = 0; i < particleCount * 3; i++) {
            const targetPos = targetArr[i] * state.expansion;
            posAttr.array[i] += (targetPos - posAttr.array[i]) * 0.1;
        }
        posAttr.needsUpdate = true;

        // AI Gesture Logic
        if (video.readyState === 4) {
            const results = handLandmarker.detectForVideo(video, performance.now());
            if (results.landmarks && results.landmarks.length > 0) {
                const hand = results.landmarks[0];
                const dx = hand[4].x - hand[8].x;
                const dy = hand[4].y - hand[8].y;
                const dist = Math.sqrt(dx*dx + dy*dy);
                
                // Ekspansi berdasarkan jarak jari (mencubit/melepas)
                state.expansion = THREE.MathUtils.mapLinear(dist, 0.03, 0.25, 0.2, 3.5);
                
                // Rotasi berdasarkan posisi telapak tangan
                particles.rotation.y += (hand[0].x - 0.5) * 0.1;
                particles.rotation.x += (hand[0].y - 0.5) * 0.1;
            } else {
                state.expansion += (1.0 - state.expansion) * 0.05;
                particles.rotation.y += state.rotationSpeed;
            }
        }

        renderer.render(scene, camera);
    }

    window.addEventListener('resize', () => {
        camera.aspect = window.innerWidth / window.innerHeight;
        camera.updateProjectionMatrix();
        renderer.setSize(window.innerWidth, window.innerHeight);
    });
</script>

<script>
    function toggleFS() {
        if (!document.fullscreenElement) {
            document.documentElement.requestFullscreen().catch(e => alert(e.message));
        } else {
            document.exitFullscreen();
        }
    }
</script>

</body>
</html>
