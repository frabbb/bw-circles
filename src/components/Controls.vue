<script setup>
import { defineModel, ref, onMounted, onUnmounted } from 'vue'
import Range from './Range.vue'

const params = defineModel()

const open = ref(true)

const container = ref(null)
let resizeObserver = null
const maxHeight = ref(0)

onMounted(() => {
  resizeObserver = new ResizeObserver((entries) => {
    for (const entry of entries) {
      const { height } = entry.target.getBoundingClientRect()
      maxHeight.value = `${height + 16}px`
    }
  })

  if (container.value) {
    resizeObserver.observe(container.value)
  }
})

onUnmounted(() => {
  if (resizeObserver) {
    resizeObserver.disconnect()
  }
})
</script>

<template>
  <div class="controls-wrapper">
    <div class="controls-container" :class="{ open }" :style="{ '--max-h': maxHeight }">
      <div ref="container">
        <div class="header">
          <p>Controls</p>
          <div class="close" :class="{ open }" @click="open = !open">
            <p>{{ open ? '-' : '+' }}</p>
          </div>
        </div>
        <Range v-for="(p, key) in params" :label="key" :min="p.min" :max="p.max" v-model="p.value">
        </Range>
      </div>
    </div>
  </div>
</template>

<style scoped>
.controls-wrapper {
  position: fixed;
  top: 0;
  right: 0;
  padding: 8px;
  width: 100%;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.close {
  width: var(--xl);
  height: var(--xl);
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}

@media screen and (min-width: 500px) {
  .controls-wrapper {
    max-width: 300px;
  }
}

.controls-container {
  background-color: rgba(200, 200, 200, 0.5);
  backdrop-filter: blur(50px);
  width: 100%;
  padding: 8px;
  border-radius: 4px;

  max-height: 40px;
  overflow: hidden;
  transition: max-height 0.15s ease-in-out;

  color: white;
}

.controls-container.open {
  max-height: var(--max-h);
}

.controls-container > div {
  display: grid;
  gap: 8px;
}

.input-container {
  display: flex;
  align-items: center;
  gap: 8px;
}

input[type='range'] {
  flex: 1;
}
</style>
