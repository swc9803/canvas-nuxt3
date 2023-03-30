<template>
  <div id="flow">
    <canvas ref="canvasRef" />
  </div>
</template>

<script setup>
const canvasRef = ref()
let ctx
let effect

class Particle {
  constructor (effect) {
    this.effect = effect
    this.x = Math.floor(Math.random() * this.effect.width)
    this.y = Math.floor(Math.random() * this.effect.height)
    this.speedX = 0
    this.speedY = 0
    this.speedModifier = Math.floor(Math.random() * 5 + 1)

    this.history = [{ x: this.x, y: this.y }]
    this.maxLength = Math.floor(Math.random() * 200 + 10)
    this.angle = 0
    this.timer = this.maxLength * 2
  }

  draw (context) {
    context.beginPath()
    context.moveTo(this.history[0].x, this.history[0].y)
    for (let i = 0; i < this.history.length; i++) {
      context.lineTo(this.history[i].x, this.history[i].y)
    }
    context.stroke()
  }

  update () {
    this.timer--
    if (this.timer >= 1) {
      const x = Math.floor(this.x / this.effect.cellSize)
      const y = Math.floor(this.y / this.effect.cellSize)
      const index = y * this.effect.cols + x
      this.angle = this.effect.flowField[index]

      this.speedX = Math.cos(this.angle)
      this.speedY = Math.sin(this.angle)
      this.x += this.speedX * this.speedModifier
      this.y += this.speedY * this.speedModifier

      this.history.push({ x: this.x, y: this.y })
      if (this.history.length > this.maxLength) {
        this.history.shift()
      }
    } else if (this.history.length > 1) {
      this.history.shift()
    } else {
      this.reset()
    }
  }

  reset () {
    this.x = Math.floor(Math.random() * this.effect.width)
    this.y = Math.floor(Math.random() * this.effect.height)
    this.history = [{ x: this.x, y: this.y }]
    this.timer = this.maxLength * 2
  }
}

class Effect {
  constructor (width, height) {
    this.width = width
    this.height = height
    this.particles = []
    this.numberOfParticles = 1000
    this.cellSize = 30
    this.rows = 0
    this.cols = 0
    this.flowField = []
    this.curve = 5
    this.zoom = 0.1
    this.debug = true
    this.init()

    window.addEventListener('keydown', (e) => {
      if (e.key === 'd') {
        this.debug = !this.debug
      }
    })
  }

  init () {
    this.rows = Math.floor(this.height / this.cellSize)
    this.cols = Math.floor(this.width / this.cellSize)
    this.flowField = []
    for (let y = 0; y < this.rows; y++) {
      for (let x = 0; x < this.cols; x++) {
        const angle = (Math.cos(x * this.zoom) + Math.sin(y * this.zoom)) * this.curve
        this.flowField.push(angle)
      }
    }

    for (let i = 0; i < this.numberOfParticles; i++) {
      this.particles.push(new Particle(this))
    }
  }

  drawGrid (context) {
    context.save()
    context.strokeStyle = 'white'
    context.lineWidth = 0.3
    for (let c = 0; c < this.cols; c++) {
      context.beginPath()
      context.moveTo(this.cellSize * c, 0)
      context.lineTo(this.cellSize * c, this.height)
      context.stroke()
    }
    for (let r = 0; r < this.rows; r++) {
      context.beginPath()
      context.moveTo(0, this.cellSize * r)
      context.lineTo(this.width, this.cellSize * r)
      context.stroke()
    }
    context.restore()
  }

  render (ctx) {
    if (this.debug) {
      this.drawGrid(ctx)
    }
    this.particles.forEach((particle) => {
      particle.draw(ctx)
      particle.update()
    })
  }
}

function animate () {
  ctx.clearRect(0, 0, canvasRef.value.width, canvasRef.value.height)
  effect.render(ctx)
  requestAnimationFrame(animate)
}

const onResize = () => {
  canvasRef.value.width = window.innerWidth
  canvasRef.value.height = window.innerHeight
}

onMounted(() => {
  ctx = canvasRef.value.getContext('2d')
  onResize()
  effect = new Effect(canvasRef.value.width, canvasRef.value.height)

  ctx.fillStyle = 'white'
  ctx.strokeStyle = 'white'
  animate()

  window.addEventListener('resize', onResize)
})
</script>
