<template>
  <div id="threejs-background" style="position: fixed; top: 0; left: 0; width: 100%; height: 100%"></div>
</template>
<!-- 
<script>
import * as THREE from 'three';

export default {
  name: 'BackgroundAnimation',
  mounted() {
    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);

    const renderer = new THREE.WebGLRenderer();
    renderer.setSize(window.innerWidth, window.innerHeight);
    document.getElementById('threejs-background').appendChild(renderer.domElement);

    const geometry = new THREE.SphereGeometry(0.5, 16, 8);

    const particles = new THREE.Group();
    scene.add(particles);

    for (let i = 0; i < 1000; i++) {
      const color = new THREE.Color(Math.random() * 0xffffff); // Génère une couleur aléatoire
      const material = new THREE.MeshBasicMaterial({ color });
      const mesh = new THREE.Mesh(geometry, material);
      mesh.position.set(Math.random() * 10 - 5, Math.random() * 10 - 5, Math.random() * 10 - 5);
      mesh.velocity = new THREE.Vector3(Math.random() * 0.02 - 0.01, Math.random() * 0.02 - 0.01, Math.random() * 0.02 - 0.01);
      particles.add(mesh);
    }

    camera.position.z = 5;

    const animate = function () {
      requestAnimationFrame(animate);

      particles.children.forEach(function (p) {
        p.position.add(p.velocity);
        if (p.position.length() > 5) {
          p.position.set(Math.random() * 10 - 5, Math.random() * 10 - 5, Math.random() * 10 - 5);
        }
      });

      renderer.render(scene, camera);
    }

    animate();

    window.addEventListener('resize', function () {
      camera.aspect = window.innerWidth / window.innerHeight;
      camera.updateProjectionMatrix();
      renderer.setSize(window.innerWidth, window.innerHeight);
    }, false);
  }
}
</script> -->
