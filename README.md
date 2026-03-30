<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>Untuk Cintaku | RYEENZY</title>
    <style>
        body {
            margin: 0;
            overflow: hidden;
            font-family: 'Poppins', 'Segoe UI', 'Montserrat', sans-serif;
            color: white;
            background-color: black;
        }

        /* Overlay tombol play awal */
        .play-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: radial-gradient(circle at center, #0a0718, #000000);
            z-index: 200;
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
            backdrop-filter: blur(8px);
            transition: opacity 1.2s ease;
            cursor: pointer;
        }
        .play-btn {
            background: rgba(0,0,0,0.5);
            border: 2px solid rgba(255, 120, 160, 0.9);
            border-radius: 80px;
            padding: 18px 60px;
            font-size: 2rem;
            font-weight: 700;
            letter-spacing: 6px;
            color: #ffc0e0;
            text-shadow: 0 0 20px #ff66aa;
            backdrop-filter: blur(12px);
            cursor: pointer;
            transition: all 0.4s ease;
            animation: pulseGlow 1.6s infinite alternate;
            font-family: 'Poppins', monospace;
            box-shadow: 0 0 30px rgba(255,80,120,0.5);
        }
        .play-btn:hover {
            transform: scale(1.08);
            background: rgba(255,80,120,0.3);
            border-color: #ff99cc;
            box-shadow: 0 0 60px rgba(255,80,120,0.8);
        }
        @keyframes pulseGlow {
            0% { text-shadow: 0 0 5px #ff88aa; box-shadow: 0 0 15px rgba(255,80,120,0.3);}
            100% { text-shadow: 0 0 25px #ff66aa; box-shadow: 0 0 50px rgba(255,80,120,0.7);}
        }
        .play-sub {
            margin-top: 30px;
            font-size: 0.9rem;
            letter-spacing: 3px;
            color: rgba(255,200,220,0.8);
            font-weight: 300;
        }

        /* Container pesan romantis (muncul setelah play) */
        .message-container {
            position: fixed;
            bottom: 15%;
            left: 0;
            width: 100%;
            text-align: center;
            z-index: 30;
            pointer-events: none;
            opacity: 0;
            transition: opacity 1.5s ease;
        }
        .romantic-message {
            display: inline-block;
            background: rgba(10, 5, 30, 0.7);
            backdrop-filter: blur(15px);
            padding: 16px 32px;
            border-radius: 60px;
            font-size: 1.7rem;
            font-weight: 500;
            letter-spacing: 1px;
            border: 1px solid rgba(255, 140, 180, 0.8);
            box-shadow: 0 0 40px rgba(255, 80, 120, 0.4);
            font-family: 'Poppins', monospace;
            color: #fff0f5;
            text-shadow: 0 0 8px #ff66aa;
            max-width: 80vw;
        }
        @media (max-width: 700px) {
            .romantic-message { font-size: 1.2rem; padding: 12px 24px; }
        }

        /* Kreator */
        .creator {
            position: fixed;
            bottom: 20px;
            left: 20px;
            z-index: 40;
            font-size: 0.8rem;
            background: rgba(0,0,0,0.5);
            backdrop-filter: blur(5px);
            padding: 5px 12px;
            border-radius: 30px;
            letter-spacing: 1px;
            font-family: monospace;
            color: #ffbbdd;
            pointer-events: none;
        }

        /* Tombol kontrol musik */
        .music-toggle {
            position: fixed;
            bottom: 20px;
            right: 20px;
            z-index: 40;
            background: rgba(0,0,0,0.6);
            backdrop-filter: blur(8px);
            border: 1px solid rgba(255,140,180,0.6);
            border-radius: 50px;
            padding: 8px 16px;
            font-size: 0.9rem;
            cursor: pointer;
            color: white;
            font-family: monospace;
            transition: 0.2s;
            pointer-events: auto;
            opacity: 0;
            transition: opacity 0.5s;
        }
        .music-toggle:hover {
            background: #ff66aa80;
            transform: scale(1.05);
        }

        /* Grain efek */
        .grain {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 10;
            background-image: url('data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIzMDAiIGhlaWdodD0iMzAwIj48ZmlsdGVyIGlkPSJmIj48ZmVUdXJidWxlbmNlIHR5cGU9ImZyYWN0YWxOb2lzZSIgYmFzZUZyZXF1ZW5jeT0iLjUiIG51bU9jdGF2ZXM9IjMiLz48L2ZpbHRlcj48cmVjdCB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIiBmaWx0ZXI9InVybCgjZikiIG9wYWNpdHk9IjAuMTUiLz48L3N2Zz4=');
            opacity: 0.2;
            mix-blend-mode: overlay;
            pointer-events: none;
        }
    </style>
</head>
<body>

    <!-- TOMBOL PLAY -->
    <div id="playOverlay" class="play-overlay">
        <div class="play-btn" id="startButton">✦ MULAI ✦</div>
        <div class="play-sub">sentuh untuk membuka hati</div>
    </div>

    <!-- Tempat pesan romantis -->
    <div id="messageContainer" class="message-container">
        <div class="romantic-message" id="romanticText">✨</div>
    </div>

    <!-- Kreator -->
    <div class="creator">CREATOR: RYEENZY ✦</div>

    <!-- Tombol musik -->
    <button id="musicToggle" class="music-toggle" style="opacity:0;">🎵 Musik</button>

    <div class="grain"></div>

    <!-- Audio -->
    <audio id="bgMusic" loop preload="auto">
        <source src="https://ryeenzyexp.edgeone.app/ssstik.io_1774844982491.mp3" type="audio/mpeg">
    </audio>

    <!-- Three.js -->
    <script type="importmap">
        {
            "imports": {
                "three": "https://unpkg.com/three@0.128.0/build/three.module.js"
            }
        }
    </script>

    <script type="module">
        import * as THREE from 'three';

        // --- Setup Scene
        const scene = new THREE.Scene();
        scene.background = new THREE.Color(0x050210);
        scene.fog = new THREE.FogExp2(0x050210, 0.0006);

        const camera = new THREE.PerspectiveCamera(45, window.innerWidth / window.innerHeight, 0.1, 1000);
        camera.position.set(0, 2, 18);
        camera.lookAt(0, 0, 0);

        const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: false });
        renderer.setSize(window.innerWidth, window.innerHeight);
        renderer.setPixelRatio(window.devicePixelRatio);
        document.body.appendChild(renderer.domElement);

        // --- Lighting
        const ambientLight = new THREE.AmbientLight(0x331122);
        scene.add(ambientLight);
        const mainLight = new THREE.PointLight(0xff88aa, 0.9);
        mainLight.position.set(2, 3, 4);
        scene.add(mainLight);
        const backLight = new THREE.PointLight(0xaa66ff, 0.6);
        backLight.position.set(-1, 1, -6);
        scene.add(backLight);
        const heartLight = new THREE.PointLight(0xff4488, 1.2);
        heartLight.position.set(0, 0.5, 2);
        scene.add(heartLight);

        // --- Background Stars (tetap diam)
        const starGeometry = new THREE.BufferGeometry();
        const starCount = 2000;
        const starPositions = new Float32Array(starCount * 3);
        for (let i = 0; i < starCount; i++) {
            starPositions[i*3] = (Math.random() - 0.5) * 400;
            starPositions[i*3+1] = (Math.random() - 0.5) * 200;
            starPositions[i*3+2] = (Math.random() - 0.5) * 100 - 50;
        }
        starGeometry.setAttribute('position', new THREE.BufferAttribute(starPositions, 3));
        const starMaterial = new THREE.PointsMaterial({ color: 0xffffff, size: 0.18, transparent: true, opacity: 0.7, blending: THREE.AdditiveBlending });
        const stars = new THREE.Points(starGeometry, starMaterial);
        scene.add(stars);

        // --- GROUP UNTUK PARTIKEL YANG BISA DIGESER
        const draggableGroup = new THREE.Group();
        scene.add(draggableGroup);

        // 1. Heart Particle (3D heart shape)
        const heartParticleCount = 4200;
        const heartGeometry = new THREE.BufferGeometry();
        const heartPositions = new Float32Array(heartParticleCount * 3);
        const heartColors = new Float32Array(heartParticleCount * 3);

        for (let i = 0; i < heartParticleCount; i++) {
            const t = Math.random() * Math.PI * 2;
            const scale = 0.22;
            const xRaw = 16 * Math.pow(Math.sin(t), 3);
            const yRaw = 13 * Math.cos(t) - 5 * Math.cos(2*t) - 2 * Math.cos(3*t) - Math.cos(4*t);
            const z = (Math.random() - 0.5) * 2.4;
            const x = (xRaw + (Math.random() - 0.5) * 0.9) * scale;
            const y = (yRaw + (Math.random() - 0.5) * 0.9) * scale;
            heartPositions[i*3] = x;
            heartPositions[i*3+1] = y;
            heartPositions[i*3+2] = z;
            
            const color = new THREE.Color();
            const mixFactor = (y + 2.5) / 5;
            if (mixFactor < 0.5) color.setHSL(0.95, 1.0, 0.6);
            else color.setHSL(0.88, 1.0, 0.55);
            heartColors[i*3] = color.r;
            heartColors[i*3+1] = color.g;
            heartColors[i*3+2] = color.b;
        }
        heartGeometry.setAttribute('position', new THREE.BufferAttribute(heartPositions, 3));
        heartGeometry.setAttribute('color', new THREE.BufferAttribute(heartColors, 3));
        const heartPointsMat = new THREE.PointsMaterial({ size: 0.08, vertexColors: true, blending: THREE.AdditiveBlending, transparent: true });
        const heartParticles = new THREE.Points(heartGeometry, heartPointsMat);
        draggableGroup.add(heartParticles);

        // 2. Sparkle particles (berkilau)
        const sparkleCount = 1800;
        const sparkleGeo = new THREE.BufferGeometry();
        const sparklePos = [];
        for (let i = 0; i < sparkleCount; i++) {
            const rad = 2.8 + Math.random() * 2;
            const theta = Math.random() * Math.PI * 2;
            const phi = Math.acos(2 * Math.random() - 1);
            const x = rad * Math.sin(phi) * Math.cos(theta);
            const y = rad * Math.sin(phi) * Math.sin(theta) * 0.8;
            const z = rad * Math.cos(phi);
            sparklePos.push(x, y + 0.2, z);
        }
        sparkleGeo.setAttribute('position', new THREE.BufferAttribute(new Float32Array(sparklePos), 3));
        const sparkleMat = new THREE.PointsMaterial({ color: 0xffaacc, size: 0.05, transparent: true, opacity: 0.6, blending: THREE.AdditiveBlending });
        const sparkles = new THREE.Points(sparkleGeo, sparkleMat);
        draggableGroup.add(sparkles);

        // 3. Orb particles
        const orbCount = 400;
        const orbGeo = new THREE.BufferGeometry();
        const orbPositions = [];
        for (let i = 0; i < orbCount; i++) {
            const radius = 3.2 + Math.random() * 2.5;
            const angle = Math.random() * Math.PI * 2;
            const height = (Math.random() - 0.5) * 3.5;
            orbPositions.push(Math.cos(angle) * radius, height, Math.sin(angle) * radius);
        }
        orbGeo.setAttribute('position', new THREE.BufferAttribute(new Float32Array(orbPositions), 3));
        const orbMat = new THREE.PointsMaterial({ color: 0xff88bb, size: 0.09, transparent: true, opacity: 0.4, blending: THREE.AdditiveBlending });
        const orbs = new THREE.Points(orbGeo, orbMat);
        draggableGroup.add(orbs);

        // ========== PARTIKEL JATUH (Falling Stars) ==========
        const fallingCount = 800;
        const fallingGeometry = new THREE.BufferGeometry();
        const fallingPositions = new Float32Array(fallingCount * 3);
        const fallingVelocities = [];
        const fallingColors = new Float32Array(fallingCount * 3);
        
        for (let i = 0; i < fallingCount; i++) {
            // Posisi acak di area lebar dan tinggi
            const x = (Math.random() - 0.5) * 20;
            const y = Math.random() * 12 - 2; // dari atas ke bawah
            const z = (Math.random() - 0.5) * 15 - 5;
            fallingPositions[i*3] = x;
            fallingPositions[i*3+1] = y;
            fallingPositions[i*3+2] = z;
            
            // Kecepatan jatuh (per detik) antara 1.5 sampai 4
            fallingVelocities.push({
                y: -(1.5 + Math.random() * 3),
                x: (Math.random() - 0.5) * 0.5,
                z: (Math.random() - 0.5) * 0.5
            });
            
            // Warna pastel: pink, ungu, putih, peach
            const colorChoice = Math.random();
            let r, g, b;
            if (colorChoice < 0.33) { r=1.0; g=0.7; b=0.9; }      // pink
            else if (colorChoice < 0.66) { r=0.9; g=0.6; b=1.0; } // ungu
            else { r=1.0; g=0.9; b=0.8; }                          // peach/putih
            fallingColors[i*3] = r;
            fallingColors[i*3+1] = g;
            fallingColors[i*3+2] = b;
        }
        fallingGeometry.setAttribute('position', new THREE.BufferAttribute(fallingPositions, 3));
        fallingGeometry.setAttribute('color', new THREE.BufferAttribute(fallingColors, 3));
        const fallingMat = new THREE.PointsMaterial({ size: 0.07, vertexColors: true, transparent: true, opacity: 0.8, blending: THREE.AdditiveBlending });
        const fallingStars = new THREE.Points(fallingGeometry, fallingMat);
        scene.add(fallingStars);
        
        // Simpan referensi untuk update posisi
        const fallingPosAttr = fallingGeometry.attributes.position;
        
        // ========== DRAG & RETURN ==========
        const originalGroupPos = new THREE.Vector3(0, 0, 0);
        let isDragging = false;
        let lastMouseX = 0, lastMouseY = 0;
        let returnAnimationId = null;
        
        function animateReturn() {
            if (!isDragging && (draggableGroup.position.x !== 0 || draggableGroup.position.y !== 0 || draggableGroup.position.z !== 0)) {
                draggableGroup.position.x *= 0.92;
                draggableGroup.position.y *= 0.92;
                draggableGroup.position.z *= 0.92;
                if (Math.abs(draggableGroup.position.x) < 0.01 && Math.abs(draggableGroup.position.y) < 0.01 && Math.abs(draggableGroup.position.z) < 0.01) {
                    draggableGroup.position.set(0, 0, 0);
                    cancelAnimationFrame(returnAnimationId);
                    returnAnimationId = null;
                } else {
                    returnAnimationId = requestAnimationFrame(animateReturn);
                }
            }
        }
        
        function onPointerDown(event) {
            isDragging = true;
            if (returnAnimationId) {
                cancelAnimationFrame(returnAnimationId);
                returnAnimationId = null;
            }
            const clientX = event.clientX ?? event.touches[0].clientX;
            const clientY = event.clientY ?? event.touches[0].clientY;
            lastMouseX = clientX;
            lastMouseY = clientY;
            event.preventDefault();
        }
        
        function onPointerMove(event) {
            if (!isDragging) return;
            const clientX = event.clientX ?? event.touches[0].clientX;
            const clientY = event.clientY ?? event.touches[0].clientY;
            const deltaX = clientX - lastMouseX;
            const deltaY = clientY - lastMouseY;
            const sensitivity = 0.008;
            draggableGroup.position.x += deltaX * sensitivity;
            draggableGroup.position.y -= deltaY * sensitivity;
            draggableGroup.position.x = Math.min(Math.max(draggableGroup.position.x, -3), 3);
            draggableGroup.position.y = Math.min(Math.max(draggableGroup.position.y, -2), 2);
            lastMouseX = clientX;
            lastMouseY = clientY;
            event.preventDefault();
        }
        
        function onPointerUp() {
            if (!isDragging) return;
            isDragging = false;
            if (returnAnimationId) cancelAnimationFrame(returnAnimationId);
            returnAnimationId = requestAnimationFrame(animateReturn);
        }
        
        renderer.domElement.addEventListener('mousedown', onPointerDown);
        window.addEventListener('mousemove', onPointerMove);
        window.addEventListener('mouseup', onPointerUp);
        renderer.domElement.addEventListener('touchstart', onPointerDown);
        window.addEventListener('touchmove', onPointerMove);
        window.addEventListener('touchend', onPointerUp);
        
        // --- Animasi
        let time = 0;
        let lastTimestamp = performance.now();
        
        // --- Pesan romantis
        const loveMessages = [
            "Untukmu, bintang di hatiku ✨",
            "Setiap detik bersamamu adalah keajaiban.",
            "Aku takut kehilanganmu, tapi aku berani mencintaimu.",
            "Kamu adalah alasan galaksi ini terang.",
            "Di antara miliaran jiwa, aku memilihmu.",
            "Jangan pernah pergi, kau rumahku.",
            "Cintaku padamu tak terukur oleh waktu.",
            "Kamu membuat hidupku berwarna pelangi.",
            "Aku berjanji akan menjagamu selamanya.",
            "Hari ini, besok, dan selamanya... milikmu."
        ];
        
        let msgIndex = 0;
        const msgContainer = document.getElementById('messageContainer');
        const msgTextElem = document.getElementById('romanticText');
        
        function startMessageSequence() {
            msgContainer.style.opacity = '1';
            function showNext() {
                if (msgIndex < loveMessages.length) {
                    msgTextElem.innerText = loveMessages[msgIndex];
                    msgIndex++;
                    setTimeout(showNext, 5000);
                } else {
                    msgTextElem.innerText = "Selamanya milikmu. ❤️";
                }
            }
            showNext();
        }
        
        // --- MUSIK & PLAY SYSTEM
        const bgMusic = document.getElementById('bgMusic');
        const playOverlay = document.getElementById('playOverlay');
        const startBtn = document.getElementById('startButton');
        const musicToggleBtn = document.getElementById('musicToggle');
        
        let experienceStarted = false;
        
        function startExperience() {
            if (experienceStarted) return;
            experienceStarted = true;
            
            bgMusic.play().then(() => {
                musicToggleBtn.style.opacity = '1';
                musicToggleBtn.innerHTML = '🔊 Musik On';
            }).catch(e => console.log("play butuh interaksi"));
            
            playOverlay.style.opacity = '0';
            setTimeout(() => {
                playOverlay.style.display = 'none';
            }, 1000);
            
            startMessageSequence();
            
            const targetPos = { x: 0, y: 0.5, z: 15 };
            const startPos = { x: camera.position.x, y: camera.position.y, z: camera.position.z };
            let progress = 0;
            function animateCamera() {
                progress += 0.02;
                if (progress <= 1) {
                    camera.position.x = startPos.x + (targetPos.x - startPos.x) * progress;
                    camera.position.y = startPos.y + (targetPos.y - startPos.y) * progress;
                    camera.position.z = startPos.z + (targetPos.z - startPos.z) * progress;
                    requestAnimationFrame(animateCamera);
                } else {
                    camera.position.set(targetPos.x, targetPos.y, targetPos.z);
                }
            }
            animateCamera();
        }
        
        startBtn.addEventListener('click', startExperience);
        
        musicToggleBtn.addEventListener('click', () => {
            if (bgMusic.paused) {
                bgMusic.play();
                musicToggleBtn.innerHTML = '🔊 Musik On';
            } else {
                bgMusic.pause();
                musicToggleBtn.innerHTML = '🎵 Musik';
            }
        });
        
        // --- ANIMASI LOOP
        function animate() {
            const now = performance.now();
            const delta = Math.min(0.033, (now - lastTimestamp) / 1000);
            lastTimestamp = now;
            
            time += 0.008;
            
            // Rotasi internal group
            heartParticles.rotation.y = Math.sin(time * 0.2) * 0.3;
            heartParticles.rotation.x = Math.sin(time * 0.15) * 0.2;
            sparkles.rotation.y = time * 0.2;
            sparkles.rotation.x = Math.sin(time * 0.25) * 0.1;
            orbs.rotation.y = time * 0.1;
            orbs.rotation.z = Math.sin(time * 0.18) * 0.1;
            stars.rotation.y += 0.0005;
            
            // Update falling particles
            const positions = fallingPosAttr.array;
            for (let i = 0; i < fallingCount; i++) {
                let y = positions[i*3+1];
                y += fallingVelocities[i].y * delta;
                // reset jika jatuh di bawah batas
                if (y < -5) {
                    y = 7;
                    // reset posisi x dan z acak
                    positions[i*3] = (Math.random() - 0.5) * 20;
                    positions[i*3+2] = (Math.random() - 0.5) * 15 - 5;
                }
                positions[i*3+1] = y;
                // pergerakan horizontal kecil
                positions[i*3] += fallingVelocities[i].x * delta;
                positions[i*3+2] += fallingVelocities[i].z * delta;
                // batasi agar tidak keluar terlalu jauh
                if (Math.abs(positions[i*3]) > 12) positions[i*3] *= -0.98;
                if (Math.abs(positions[i*3+2]) > 12) positions[i*3+2] *= -0.98;
            }
            fallingPosAttr.needsUpdate = true;
            
            // Pulsing light
            heartLight.intensity = 1.0 + Math.sin(time * 3) * 0.4;
            mainLight.intensity = 0.8 + Math.sin(time * 2.2) * 0.2;
            
            // Kamera
            if (!isDragging && experienceStarted) {
                camera.position.x += (0 - camera.position.x) * 0.02;
                camera.position.y += (0.5 - camera.position.y) * 0.02;
                camera.position.z += (15 - camera.position.z) * 0.02;
                camera.lookAt(0, 0.2, 0);
            } else {
                camera.lookAt(0, 0.2, 0);
            }
            
            renderer.render(scene, camera);
            requestAnimationFrame(animate);
        }
        
        animate();
        
        window.addEventListener('resize', () => {
            camera.aspect = window.innerWidth / window.innerHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(window.innerWidth, window.innerHeight);
        });
        
        console.log("✨ Partikel jatuh ditambahkan!");
    </script>
</body>
</html>
