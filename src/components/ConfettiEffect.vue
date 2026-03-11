<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const canvasRef = ref(null)
let animationId = null
let startTime = null

const COLORS = ['#D4AF37', '#F0D060', '#8B6914', '#C8A850', '#E8C84A', '#B8960C']

function randomBetween(a, b) {
  return a + Math.random() * (b - a)
}

function createParticle(canvas) {
  return {
    x: randomBetween(0, canvas.width),
    y: randomBetween(-20, -5),
    vx: randomBetween(-1.2, 1.2),
    vy: randomBetween(2.5, 5.5),
    rotation: randomBetween(0, Math.PI * 2),
    rotationSpeed: randomBetween(-0.08, 0.08),
    width: randomBetween(8, 16),
    height: randomBetween(4, 9),
    color: COLORS[Math.floor(Math.random() * COLORS.length)],
    shape: Math.random() > 0.5 ? 'rect' : 'circle',
    opacity: 1,
  }
}

function start() {
  const canvas = canvasRef.value
  if (!canvas) return

  canvas.width = window.innerWidth
  canvas.height = window.innerHeight

  const ctx = canvas.getContext('2d')
  const particles = Array.from({ length: 80 }, () => createParticle(canvas))
  startTime = performance.now()

  const DURATION = 4000

  function draw(now) {
    const elapsed = now - startTime
    const progress = Math.min(elapsed / DURATION, 1)

    ctx.clearRect(0, 0, canvas.width, canvas.height)

    const globalAlpha = progress > 0.7
      ? 1 - (progress - 0.7) / 0.3
      : 1

    particles.forEach((p) => {
      // Physics
      p.x += p.vx + Math.sin(elapsed * 0.001 + p.y * 0.01) * 0.5
      p.y += p.vy
      p.rotation += p.rotationSpeed
      p.vy += 0.05 // gravity

      // Reset if off screen
      if (p.y > canvas.height + 20) {
        Object.assign(p, createParticle(canvas))
        p.y = -20
      }

      ctx.save()
      ctx.globalAlpha = globalAlpha * p.opacity
      ctx.translate(p.x, p.y)
      ctx.rotate(p.rotation)
      ctx.fillStyle = p.color

      if (p.shape === 'rect') {
        ctx.fillRect(-p.width / 2, -p.height / 2, p.width, p.height)
      } else {
        ctx.beginPath()
        ctx.ellipse(0, 0, p.width / 2, p.height / 2, 0, 0, Math.PI * 2)
        ctx.fill()
      }
      ctx.restore()
    })

    if (progress < 1) {
      animationId = requestAnimationFrame(draw)
    } else {
      ctx.clearRect(0, 0, canvas.width, canvas.height)
    }
  }

  if (animationId) cancelAnimationFrame(animationId)
  animationId = requestAnimationFrame(draw)
}

onUnmounted(() => {
  if (animationId) cancelAnimationFrame(animationId)
})

defineExpose({ start })
</script>

<template>
  <canvas ref="canvasRef" class="confetti-canvas"></canvas>
</template>

<style scoped>
.confetti-canvas {
  position: fixed;
  inset: 0;
  pointer-events: none;
  z-index: 500;
}
</style>
