<template>
  <div class="three-js">
    <h1>Hello</h1>
  </div>
</template>

<script>
import * as THREE from "three";
export default {
  mounted: function () {
    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);

    const renderer = new THREE.WebGLRenderer();
    renderer.setSize(window.innerWidth, window.innerHeight);
    document.body.appendChild(renderer.domElement);

    const curve = new THREE.EllipseCurve(
      0,
      0, // ax, aY
      10,
      10, // xRadius, yRadius
      0,
      2 * Math.PI, // aStartAngle, aEndAngle
      false, // aClockwise
      0 // aRotation
    );

    const points = curve.getPoints(50);
    const geometry = new THREE.BufferGeometry().setFromPoints(points);

    const material = new THREE.LineBasicMaterial({ color: 0xff0000 });

    // Create the final object to add to the scene
    const ellipse = new THREE.Line(geometry, material);

    camera.position.z = 5;

    function animate() {
      requestAnimationFrame(animate);
      ellipse.rotation.x += 0.01;
      ellipse.rotation.y += 0.01;
      renderer.render(scene, camera);
    }
    animate();
  },
};
</script>
