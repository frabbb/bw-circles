<script setup>
import { ref, defineProps } from 'vue'
import p5 from 'p5'

const props = defineProps({
  params: {
    type: Object,
    default: () => ({}),
  },
})

const container = ref(null)

const sk = new p5((p) => p)
let canvas

window.addEventListener('resize', resize)

sk.setup = () => {
  canvas = sk.createCanvas(sk.windowWidth, sk.windowHeight)
  container.value.appendChild(canvas.elt)
  sk.angleMode(sk.DEGREES)
  sk.strokeWeight(0)
}

sk.draw = () => {
  sk.background(0)

  const { n, speed, size, alpha, arc_width } = props.params

  for (let i = 0; i < n; i++) {
    sk.push()
    const maxRadius = (Math.min(sk.width, sk.height) * size) / 100
    const minRadius = 10

    const radius = sk.map(i, 0, n, maxRadius, minRadius)
    // const speed = sk.map(i + 1, 0, n, 1, maxSpeed) * (Math.sin(sk.frameCount / 1000) + 1)

    const rotationAngle = (sk.frameCount / 10) * (i + 1) * speed

    sk.translate(sk.width / 2, sk.height / 2)
    sk.rotate(rotationAngle)

    sk.fill(0, 0, 0, alpha)
    sk.arc(0, 0, radius, radius, 0, arc_width)

    sk.fill(255, 255, 255, alpha)
    sk.arc(0, 0, radius, radius, arc_width, 360)

    sk.pop()
  }
}

function resize() {
  sk.resizeCanvas(sk.windowWidth, sk.windowHeight)
}
</script>

<template>
  <div class="canvas-container" ref="container"></div>
</template>

<style scoped></style>
