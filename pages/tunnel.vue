<template>
  <div id="tunnel" ref="containerRef" class="container">
    <canvas />
  </div>
</template>

<script setup>
import * as THREE from 'three'

const containerRef = ref()
let camera
let renderer

const scene = new THREE.Scene()
scene.fog = new THREE.Fog(0x000000, 100, 200)

// <polygon points="68.5,185.5 1,262.5 270.9,281.9 345.5,212.8 178,155.7 240.3,72.3 153.4,0.6 52.6,53.3 "/>
const points = [
  [68.5, 185.5],
  [1, 262.5],
  [270.9, 281.9],
  [300, 212.8],
  [178, 155.7],
  [240.3, 72.3],
  [153.4, 0.6],
  [52.6, 53.3],
  [68.5, 185.5]
]

// Convert the array of points into vertices
for (let i = 0; i < points.length; i++) {
  const x = points[i][0]
  const y = (Math.random() - 0.5) * 250
  const z = points[i][1]
  points[i] = new THREE.Vector3(x, y, z)
}
// Create a path from the points
const path = new THREE.CatmullRomCurve3(points)

const colors = [0xFF6138, 0xFFFF9D, 0xBEEB9F, 0x79BD8F, 0x00A388]
for (let i = 0; i < colors.length; i++) {
  const geometry = new THREE.TubeGeometry(path, 100, (i * 2) + 4, 10, true)
  const material = new THREE.MeshBasicMaterial({
    color: colors[i],
    transparent: true,
    wireframe: true,
    opacity: ((1 - i / 5) * 0.5 + 0.1)
  })
  const tube = new THREE.Mesh(geometry, material)
  scene.add(tube)
}

let percentage = 0
function animate () {
  percentage += 0.0003
  const p1 = path.getPointAt(percentage % 1)
  const p2 = path.getPointAt((percentage + 0.01) % 1)
  camera.position.set(p1.x, p1.y, p1.z)
  camera.lookAt(p2)

  renderer.render(scene, camera)

  requestAnimationFrame(animate)
}

function init () {
  renderer = new THREE.WebGLRenderer({
    antialias: true
    // alpha: true
  })
  renderer.setPixelRatio(window.devicePixelRatio)
  renderer.setSize(
    containerRef.value.offsetWidth,
    containerRef.value.offsetHeight
  )
  containerRef.value.appendChild(renderer.domElement)
}

onMounted(() => {
  camera = new THREE.PerspectiveCamera(
    75,
    containerRef.value.offsetWidth / containerRef.value.offsetHeight,
    0.1,
    1000
  )

  init()
  animate()
})
</script>

<style lang="scss" scoped>
canvas{
  position: absolute;
  top: 0;
  left: 0;
}

.container {
    position: absolute;
    width: 100%;
    height: 100%;
}
</style>
