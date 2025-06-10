<script setup>
import { ref, computed } from 'vue'
import Canvas from './components/Canvas.vue'
import Controls from './components/Controls.vue'

const params = ref({
  size: { min: 1, max: 200, value: 90 },
  n: { min: 1, max: 100, value: 20 },
  speed: { min: 1, max: 10, value: 1 },
  alpha: { min: 0, max: 255, value: 255 },
  arc_width: { min: 0, max: 360, value: 180 },
})

const values = computed(() =>
  Object.fromEntries(Object.entries(params.value).map(([key, { value }]) => [key, value])),
)
</script>

<template>
  <Canvas :params="values" />
  <Controls v-model="params" />
</template>

<style scoped></style>
