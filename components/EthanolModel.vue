<script setup>
import { onMounted, ref, onUnmounted, watch } from 'vue'
import * as THREE from 'three'

const props = defineProps({
  size: { type: Number, default: 400 },
})

const container = ref(null)
let renderer, scene, camera, ethanol, animationId
let clock = new THREE.Clock()
let animationStarted = false
let animationStartTime = 0

const createLabelSprite = (letter, atomColor) => {
  const canvas = document.createElement('canvas')
  canvas.width = 256
  canvas.height = 256
  const ctx = canvas.getContext('2d')
  ctx.clearRect(0, 0, 256, 256)
  ctx.fillStyle = atomColor === '#ffffff' ? '#000000' : '#ffffff'
  ctx.font = 'bold 160px Arial'
  ctx.textAlign = 'center'
  ctx.textBaseline = 'middle'
  ctx.fillText(letter, 128, 128)

  const texture = new THREE.CanvasTexture(canvas)
  const spriteMaterial = new THREE.SpriteMaterial({ 
    map: texture, 
    transparent: true, 
    depthTest: false 
  })
  const sprite = new THREE.Sprite(spriteMaterial)
  sprite.scale.set(0.6, 0.6, 1)
  return sprite
}

onMounted(() => {
  scene = new THREE.Scene()
  camera = new THREE.PerspectiveCamera(75, 1, 0.1, 1000)
  renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true })
  renderer.setSize(props.size, props.size)
  renderer.setDrawingBufferSize(1920, 1080, 1)
  container.value.appendChild(renderer.domElement)

  const light = new THREE.AmbientLight(0xffffff, 2.5)
  scene.add(light)

  ethanol = new THREE.Group()
  scene.add(ethanol)

  const atomsData = [
    { type: 'C', pos: [-1, 0, 0], color: '#333333' }, 
    { type: 'H', pos: [-1, 1, 0], color: '#ffffff' },
    { type: 'H', pos: [-2, 0, 0], color: '#ffffff' },
    { type: 'H', pos: [-1, -1, 0], color: '#ffffff' },
    
    { type: 'C', pos: [0, 0, 0], color: '#333333' }, 
    { type: 'H', pos: [0, 1, 0], color: '#ffffff' },
    { type: 'H', pos: [0, -1, 0], color: '#ffffff' },
    
    { type: 'O', pos: [1, 0, 0], color: '#ff0000' }, 
    { type: 'H', pos: [2, 0, 0], color: '#ffffff' }
  ]
  
  const atomMeshes = []
  atomsData.forEach((a, i) => {
    const size = a.type === 'H' ? 0.25 : 0.4
    const sphere = new THREE.Mesh(
      new THREE.SphereGeometry(size, 32, 32), 
      new THREE.MeshBasicMaterial({ color: a.color })
    )
    
    // Set initial state for animation (at center, scale 0)
    sphere.position.set(0, 0, 0)
    sphere.scale.set(0, 0, 0)
    
    const label = createLabelSprite(a.type, a.color)
    sphere.add(label)
    
    ethanol.add(sphere)
    atomMeshes.push({ mesh: sphere, targetPos: new THREE.Vector3(...a.pos), delay: i * 0.1 })
  })

  // Bonds
  const bondMeshes = []
  const connections = [[0,1],[0,2],[0,3],[0,4],[4,5],[4,6],[4,7],[7,8]]
  const matBond = new THREE.MeshBasicMaterial({ color: 0x555555, transparent: true, opacity: 0 })
  
  connections.forEach(conn => {
    const start = atomsData[conn[0]].pos
    const end = atomsData[conn[1]].pos
    const startVec = new THREE.Vector3(...start), endVec = new THREE.Vector3(...end)
    const bond = new THREE.Mesh(new THREE.CylinderGeometry(0.03, 0.03, startVec.distanceTo(endVec), 8), matBond.clone())
    bond.position.copy(startVec.clone().lerp(endVec, 0.5))
    bond.quaternion.setFromUnitVectors(new THREE.Vector3(0, 1, 0), endVec.clone().sub(startVec).normalize())
    
    ethanol.add(bond)
    bondMeshes.push(bond)
  })

  camera.position.z = 4

  // Watch for slidev-vclick-target class changes (like v-mark does)
  let mutationObserver = null
  
  const checkVisibility = () => {
    if (!animationStarted && container.value) {
      // Check multiple possible parent structures
      const parent = container.value.parentElement
      const vClickTarget = parent?.classList.contains('slidev-vclick-target') ? parent : 
                          container.value.closest('.slidev-vclick-target')
      
      // Check if visible through various means
      const isVisible = vClickTarget?.classList.contains('slidev-vclick-current') ||
                       vClickTarget?.classList.contains('slidev-vclick-prior') ||
                       !vClickTarget // If no v-click wrapper, show immediately
      
      if (isVisible) {
        animationStarted = true
        animationStartTime = clock.getElapsedTime()
      }
    }
  }

  // Wait for next tick to ensure DOM is ready
  setTimeout(() => {
    const parent = container.value?.parentElement
    const vClickTarget = parent?.classList.contains('slidev-vclick-target') ? parent : 
                        container.value?.closest('.slidev-vclick-target')
    
    if (vClickTarget) {
      mutationObserver = new MutationObserver(checkVisibility)
      mutationObserver.observe(vClickTarget, {
        attributes: true,
        attributeFilter: ['class']
      })
    }
    
    // Check immediately
    checkVisibility()
  }, 0)

  const animate = () => {
    animationId = requestAnimationFrame(animate)
    
    if (!animationStarted) {
      renderer.render(scene, camera)
      return
    }

    const elapsed = clock.getElapsedTime() - animationStartTime

    // Animate Atoms
    atomMeshes.forEach((item) => {
      const t = Math.max(0, Math.min(1, (elapsed - item.delay) * 1.5))
      const ease = t === 1 ? 1 : 1 - Math.pow(2, -10 * t)
      
      item.mesh.position.lerpVectors(new THREE.Vector3(0,0,0), item.targetPos, ease)
      item.mesh.scale.set(ease, ease, ease)
    })

    // Fade in Bonds
    if (elapsed > 1.5) {
      bondMeshes.forEach(b => {
        if (b.material.opacity < 1) b.material.opacity += 0.02
      })
    }

    renderer.render(scene, camera)
  }
  animate()

  onUnmounted(() => {
    if (mutationObserver) mutationObserver.disconnect()
  })
})

watch(() => props.size, (val) => renderer.setSize(val, val))
onUnmounted(() => cancelAnimationFrame(animationId))
</script>

<template>
  <div ref="container" class="inline-block"></div>
</template>
