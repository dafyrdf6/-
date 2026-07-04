<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Daflyght 3D</title>

<style>
body {
  margin: 0;
  overflow: hidden;
  background: radial-gradient(circle at center, #0b1020, #05070c);
  font-family: Inter, sans-serif;
}

#ui {
  position: absolute;
  top: 30px;
  left: 40px;
  color: white;
  z-index: 10;
}

h1 {
  font-weight: 300;
  letter-spacing: 2px;
  margin: 0;
}

p {
  color: #aaa;
  margin-top: 8px;
}

.badge {
  margin-top: 15px;
  display: inline-block;
  padding: 8px 14px;
  border: 1px solid rgba(255,255,255,0.2);
  border-radius: 20px;
  color: #fff;
  font-size: 12px;
}
</style>
</head>

<body>

<div id="ui">
  <h1>Daflyght Airlines</h1>
  <p>The Reach of Excellence</p>
  <div class="badge">Drag = Rotate • Scroll = Zoom</div>
</div>

<script src="https://cdn.jsdelivr.net/npm/three@0.160.0/build/three.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/three@0.160.0/examples/js/controls/OrbitControls.js"></script>

<script>
const scene = new THREE.Scene();
scene.fog = new THREE.Fog(0x05070c, 10, 40);

// Camera
const camera = new THREE.PerspectiveCamera(
  60,
  window.innerWidth / window.innerHeight,
  0.1,
  1000
);
camera.position.set(3, 2, 6);

// Renderer
const renderer = new THREE.WebGLRenderer({ antialias: true });
renderer.setSize(window.innerWidth, window.innerHeight);
renderer.setPixelRatio(window.devicePixelRatio);
document.body.appendChild(renderer.domElement);

// Controls (interaction souris)
const controls = new THREE.OrbitControls(camera, renderer.domElement);
controls.enableDamping = true;
controls.dampingFactor = 0.05;

// Lights (style premium)
const light1 = new THREE.DirectionalLight(0xffffff, 1.2);
light1.position.set(5, 5, 5);
scene.add(light1);

const light2 = new THREE.AmbientLight(0x4466ff, 0.3);
scene.add(light2);

// ===== AVION (placeholder élégant) =====
const group = new THREE.Group();

// fuselage
const body = new THREE.Mesh(
  new THREE.CylinderGeometry(0.2, 0.3, 4, 32),
  new THREE.MeshStandardMaterial({ color: 0xffffff, metalness: 0.6, roughness: 0.2 })
);
body.rotation.z = Math.PI / 2;
group.add(body);

// aile
const wing = new THREE.Mesh(
  new THREE.BoxGeometry(2.5, 0.05, 0.6),
  new THREE.MeshStandardMaterial({ color: 0x888888, metalness: 0.5 })
);
wing.position.y = 0;
group.add(wing);

// queue
const tail = new THREE.Mesh(
  new THREE.BoxGeometry(0.6, 0.8, 0.05),
  new THREE.MeshStandardMaterial({ color: 0xffffff })
);
tail.position.set(-2, 0.4, 0);
group.add(tail);

scene.add(group);

// PARTICLES (nuage luxe)
const particlesGeometry = new THREE.BufferGeometry();
const count = 200;

const positions = [];
for (let i = 0; i < count; i++) {
  positions.push(
    (Math.random() - 0.5) * 20,
    (Math.random() - 0.5) * 20,
    (Math.random() - 0.5) * 20
  );
}

particlesGeometry.setAttribute(
  "position",
  new THREE.Float32BufferAttribute(positions, 3)
);

const particles = new THREE.Points(
  particlesGeometry,
  new THREE.PointsMaterial({ color: 0x8899ff, size: 0.05 })
);

scene.add(particles);

// animation
function animate() {
  requestAnimationFrame(animate);

  group.rotation.y += 0.003;
  particles.rotation.y += 0.0005;

  controls.update();
  renderer.render(scene, camera);
}

animate();

// resize
window.addEventListener("resize", () => {
  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(window.innerWidth, window.innerHeight);
});
</script>

</body>
</html>
