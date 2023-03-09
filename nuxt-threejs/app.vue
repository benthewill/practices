<template>
  <div>
    <canvas id="c"></canvas>
  </div>
</template>
<script setup>
  import * as THREE from 'three'

  function main() {

    function render(time) {
      time *= 0.001 // Convert time to seconds

      if (resizeRendererToDisplaySize(renderer)) {
        const canvas = renderer.domElement
        camera.aspect = canvas.clientWidth / canvas.clientHeight
        camera.updateProjectionMatrix()
      }


      cubes.forEach((cube, ndx) => {
        const speed = 1 + ndx * .1
        const rot = time * speed
        cube.rotation.x = rot
        cube.rotation.y = rot
      })

      renderer.render(scene, camera)

      requestAnimationFrame(render)
    }

    function resizeRendererToDisplaySize(renderer) {
      const canvas = renderer.domElement
      const width = canvas.clientWidth
      const height = canvas.clientHeight
      const needResize = canvas.width !== width || canvas.height !== height
      if (needResize) {
        renderer.setSize(width, height, false)
      }
      return needResize
    }

    function makeInstance(geometry, color, x) {
      const material = new THREE.MeshPhongMaterial({color});
      const cube = new THREE.Mesh(geometry, material);
      scene.add(cube);

      cube.position.x = x;

      return cube;
    }

    const canvas = document.querySelector('#c')
    const renderer = new THREE.WebGLRenderer({
      antialias: true,
      canvas
    })

    const fov = 75;
    const aspect = 2;  // the canvas default
    const near = 0.1;
    const far = 5;
    const camera = new THREE.PerspectiveCamera(fov, aspect, near, far);

    camera.position.z = 2 // Move the camera back a little.

    const scene = new THREE.Scene()

    const boxWidth = 1
    const boxHeight = 1
    const boxDepth = 1
    const geometry = new THREE.BoxGeometry(boxWidth, boxHeight, boxDepth)

    const cubes = [
      makeInstance(geometry, 0x44aa88, 0),
      makeInstance(geometry, 0x8844aa, -2),
      makeInstance(geometry, 0xaa8844, 2)
    ]

    const color = 0XFFFFFF
    const intensity = 1
    const light = new THREE.DirectionalLight(color, intensity)
    light.position.set(-1, 2, 4) // Default position is 0,0,0
    scene.add(light)

    renderer.render(scene, camera)
    requestAnimationFrame(render)
  }

  onMounted(() => {
    main()
  })

</script>
<style>
  html, body{
    height: 100%;
  }
  #c{
    width: 100%;
    height: 100%;
    display: block;
  }
</style>
