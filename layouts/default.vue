<template>


  <div class="layout">

    <slot/>


    <canvas class="webgl"></canvas>
  </div>


</template>

<script setup>
import * as THREE from 'three'

onMounted(() => {
  console.log('layout rendered')
  let canvas = document.querySelector('canvas.webgl')

  if(!canvas.classList.contains('rendered')) renderWebgl()
})


const renderWebgl = () => {
console.log('webgl render')

  THREE.ColorManagement.enabled = false


  /**
   * Base
   */

  // Canvas
  let canvas = document.querySelector('canvas.webgl')
  const sizes = {
    width: window.innerWidth,
    height: window.innerHeight
  }


  // Scene
  const scene = new THREE.Scene()


  const geometry = new THREE.BoxGeometry( 1, 1, 1 );
  const material = new THREE.MeshStandardMaterial( { color: '#eee' } );
  const cube = new THREE.Mesh( geometry, material );
  scene.add( cube );


  /**
   * Lights
   */
  const ambiantLight = new THREE.AmbientLight('#ffffff', .6)
  const directionalLight = new THREE.DirectionalLight('#ffffff', 1)
  directionalLight.position.set(1, 2, 3)
  scene.add(directionalLight, ambiantLight)

  /**
   * Camera
   */

  // Base camera
  const camera = new THREE.PerspectiveCamera(75, sizes.width / sizes.height, 0.1, 100)
  camera.position.z = 3
  scene.add(camera)

  // Controls
  //   const controls = new OrbitControls(camera, canvas)
  //   controls.enableDamping = true


  /**
   * Cursor
   */
  const cursor = {}
  cursor.x = 0
  cursor.y = 0

  window.addEventListener('mousemove', (event) =>
  {
    cursor.x = event.clientX / sizes.width - 0.5
    cursor.y = event.clientY / sizes.height - 0.5
  })


  /**
   * Renderer
   */
  const renderer = new THREE.WebGLRenderer({
    canvas: canvas,
    alpha: true
  })
  renderer.outputColorSpace = THREE.LinearSRGBColorSpace
  renderer.setSize(sizes.width, sizes.height)
  renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))


  /* Scroll */


  /**
   * Animate
   */
  const clock = new THREE.Clock()
  let previousTime = 0
  let tickStarted = false

  const tick = () => {
    const elapsedTime = clock.getElapsedTime()
    const deltaTime = elapsedTime - previousTime
    previousTime = elapsedTime

    tickStarted = true


    // Animate model
    cube.rotation.x += deltaTime * 0.10;
    cube.rotation.y += deltaTime * 0.12;

    // Update controls
    // controls.update()

    // Animate camera
    const parallaxX = cursor.x * 0.5
    const parallaxY = - cursor.y * 0.5
    camera.position.x += (parallaxX - camera.position.x) * 5 * deltaTime
    camera.position.y += (parallaxY - camera.position.y) * 5 * deltaTime

    // Render
    renderer.render(scene, camera)

    // Call tick again on the next frame
    window.requestAnimationFrame(tick)
  }

  if (!tickStarted) tick()

  canvas.classList.add('rendered')

}


</script>
<style lang="scss" scoped>

canvas.webgl {
  @apply bg-gray-100;
  position: fixed;
  pointer-events: none;
  z-index: -1;
  top: 0;
  left: 0;
  outline: none;
}


</style>

