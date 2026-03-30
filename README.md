import * as THREE from 'three';
import { GUI } from 'lil-gui';
import { HandLandmarker, FilesetResolver } from '@mediapipe/tasks-vision';

class ParticleSystem {
  constructor() {
    this.initScene();
    this.initParticles();
    this.initHandTracking();
    this.initUI();
    this.animate();
  }

  initScene() {
    this.scene = new THREE.Scene();
    this.camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
    this.camera.position.z = 5;
    this.renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
    this.renderer.setSize(window.innerWidth, window.innerHeight);
    document.body.appendChild(this.renderer.domElement);
  }

  initParticles() {
    this.particleCount = 15000;
    this.geometry = new THREE.BufferGeometry();
    const positions = new Float32Array(this.particleCount * 3);
    const colors = new Float32Array(this.particleCount * 3);

    for (let i = 0; i < this.particleCount * 3; i++) {
      positions[i] = (Math.random() - 0.5) * 10;
    }

    this.geometry.setAttribute('position', new THREE.BufferAttribute(positions, 3));
    this.material = new THREE.PointsMaterial({
      size: 0.015,
      vertexColors: true,
      transparent: true,
      blending: THREE.AdditiveBlending
    });

    this.points = new THREE.Points(this.geometry, this.material);
    this.scene.add(this.points);
  }

  // Mediapipe Hand Tracking Setup
  async initHandTracking() {
    const vision = await FilesetResolver.forVisionTasks("https://cdn.jsdelivr.net/npm/@mediapipe/tasks-vision@latest/wasm");
    this.handLandmarker = await HandLandmarker.createFromOptions(vision, {
      baseOptions: { modelAssetPath: `https://storage.googleapis.com/mediapipe-models/hand_landmarker/hand_landmarker/float16/1/hand_landmarker.task` },
      runningMode: "VIDEO",
      numHands: 1
    });
    
    this.video = document.createElement('video');
    navigator.mediaDevices.getUserMedia({ video: true }).then((stream) => {
      this.video.srcObject = stream;
      this.video.play();
    });
  }

  // Core Interaction Logic
  updateParticlesWithGesture(results) {
    if (results.landmarks && results.landmarks.length > 0) {
      const hand = results.landmarks[0];
      // Calculate distance between thumb tip (4) and index tip (8)
      const dx = hand[4].x - hand[8].x;
      const dy = hand[4].y - hand[8].y;
      const distance = Math.sqrt(dx*dx + dy*dy);

      // Map gesture to particle behavior
      // distance < 0.1 is "Closed", > 0.4 is "Fully Open"
      const scale = THREE.MathUtils.mapLinear(distance, 0.1, 0.4, 0.2, 2.0);
      this.points.scale.set(scale, scale, scale);
      
      // Dynamic Rotation based on hand position
      this.points.rotation.y = hand[0].x * Math.PI;
      this.points.rotation.x = hand[0].y * Math.PI;
    }
  }

  initUI() {
    const gui = new GUI();
    const params = {
      color: '#00ffff',
      pattern: 'Sphere',
      fullscreen: () => document.documentElement.requestFullscreen()
    };

    gui.addColor(params, 'color').onChange(val => {
      this.material.color.set(val);
    });

    gui.add(params, 'pattern', ['Sphere', 'Cube', 'Torus', 'Cloud']);
    gui.add(params, 'fullscreen').name('Enter Fullscreen');
  }

  animate() {
    requestAnimationFrame(() => this.animate());
    
    if (this.video && this.video.readyState >= 2) {
      const results = this.handLandmarker.detectForVideo(this.video, performance.now());
      this.updateParticlesWithGesture(results);
    }

    this.renderer.render(this.scene, this.camera);
  }
}

new ParticleSystem();
