<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Ryeenzy Particle System PRO</title>

<style>
body {
  margin: 0;
  overflow: hidden;
  background: radial-gradient(circle, #020111, #000);
  font-family: Arial;
}

/* UI PANEL */
.ui {
  position: absolute;
  top: 20px;
  left: 20px;
  backdrop-filter: blur(20px);
  background: rgba(255,255,255,0.05);
  border-radius: 15px;
  padding: 15px;
  color: white;
  width: 200px;
}

.ui input, .ui select {
  width: 100%;
  margin-top: 5px;
  margin-bottom: 10px;
}

/* Webcam preview */
video {
  position: absolute;
  bottom: 20px;
  right: 20px;
  width: 150px;
  border-radius: 10px;
  opacity: 0.7;
}
</style>
</head>

<body>

<div class="ui">
  <label>Pattern</label>
  <select id="pattern">
    <option value="sphere">Sphere</option>
    <option value="galaxy">Galaxy</option>
    <option value="wave">Wave</option>
    <option value="heart">Heart</option>
  </select>

  <label>Color</label>
  <input type="color" id="color" value="#ffffff">

  <label>Size</label>
  <input type="range" id="size" min="1" max="5" value="2">

  <button onclick="toggleFullscreen()">Fullscreen</button>
</div>

<video id="webcam" autoplay playsinline></video>

<script src="https://cdn.jsdelivr.net/npm/three@0.160/build/three.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/@mediapipe/hands/hands.js"></script>
<script src="https://cdn.jsdelivr.net/npm/@mediapipe/camera_utils/camera_utils.js"></script>

<script>
/* THREE SETUP */
const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(75, innerWidth/innerHeight, 0.1, 1000);
camera.position.z = 50;

const renderer = new THREE.WebGLRenderer({antialias:true});
renderer.setSize(innerWidth, innerHeight);
document.body.appendChild(renderer.domElement);

/* PARTICLES */
let count = 8000;
let geometry = new THREE.BufferGeometry();
let positions = new Float32Array(count * 3);

let material = new THREE.PointsMaterial({
  size: 2,
  color: 0xffffff
});

let points = new THREE.Points(geometry, material);
scene.add(points);

let targetPositions = new Float32Array(count * 3);

/* SHAPES */
function sphere() {
  for(let i=0;i<count;i++){
    let r = 20 * Math.random();
    let theta = Math.random() * Math.PI * 2;
    let phi = Math.random() * Math.PI;

    targetPositions[i*3] = r * Math.sin(phi) * Math.cos(theta);
    targetPositions[i*3+1] = r * Math.sin(phi) * Math.sin(theta);
    targetPositions[i*3+2] = r * Math.cos(phi);
  }
}

function galaxy() {
  for(let i=0;i<count;i++){
    let angle = i * 0.1;
    let radius = i * 0.01;

    targetPositions[i*3] = Math.cos(angle) * radius * 5;
    targetPositions[i*3+1] = (Math.random()-0.5)*5;
    targetPositions[i*3+2] = Math.sin(angle) * radius * 5;
  }
}

function wave() {
  for(let i=0;i<count;i++){
    let x = (i % 100) - 50;
    let y = Math.floor(i/100) - 50;

    targetPositions[i*3] = x;
    targetPositions[i*3+1] = Math.sin(x*0.3) * 5;
    targetPositions[i*3+2] = y;
  }
}

function heart() {
  for (let i = 0; i < count; i++) {
    let t = Math.random() * Math.PI * 2;
    let x = 16 * Math.pow(Math.sin(t), 3);
    let y = 13 * Math.cos(t) - 5*Math.cos(2*t) - 2*Math.cos(3*t) - Math.cos(4*t);

    targetPositions[i*3] = x;
    targetPositions[i*3+1] = y;
    targetPositions[i*3+2] = 0;
  }
}

sphere();

/* UPDATE */
function animate() {
  requestAnimationFrame(animate);

  for(let i=0;i<count*3;i++){
    positions[i] += (targetPositions[i] - positions[i]) * speed;
  }

  geometry.setAttribute('position', new THREE.BufferAttribute(positions,3));

  points.rotation.y += 0.002;
  renderer.render(scene,camera);
}
animate();

/* UI */
document.getElementById("pattern").onchange = (e)=>{
  let val = e.target.value;
  if(val==="sphere") sphere();
  if(val==="galaxy") galaxy();
  if(val==="wave") wave();
  if(val==="heart") heart();
};

document.getElementById("color").oninput = (e)=>{
  material.color.set(e.target.value);
};

document.getElementById("size").oninput = (e)=>{
  material.size = e.target.value;
};

/* FULLSCREEN */
function toggleFullscreen(){
  if(!document.fullscreenElement){
    document.documentElement.requestFullscreen();
  } else {
    document.exitFullscreen();
  }
}

/* GESTURE DETECTION */
let speed = 0.05;

const video = document.getElementById("webcam");

const hands = new Hands({
  locateFile: file => `https://cdn.jsdelivr.net/npm/@mediapipe/hands/${file}`
});

hands.setOptions({
  maxNumHands: 1,
  minDetectionConfidence: 0.7,
  minTrackingConfidence: 0.7
});

hands.onResults(results=>{
  if(results.multiHandLandmarks.length > 0){
    let lm = results.multiHandLandmarks[0];

    let open = lm[8].y < lm[6].y; // simple open detect

    if(open){
      speed = 0.1; // expand
    } else {
      speed = 0.02; // contract
    }
  }
});

const cam = new Camera(video, {
  onFrame: async () => {
    await hands.send({image: video});
  },
  width: 640,
  height: 480
});
cam.start();

/* RESIZE */
window.addEventListener("resize",()=>{
  camera.aspect = innerWidth/innerHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(innerWidth,innerHeight);
});
</script>

</body>
</html>
