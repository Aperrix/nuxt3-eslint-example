<template>
  <p v-text="`Auto increment : ${autoCount}`" />
  <p v-text="`Manual increment : ${counter} x ${step || 1} = ${multiply}`" />
  <button v-text="'Increment'" @click="increment(step)" />
</template>

<script lang="ts">
const useAutoIncrement = (startCount = 0) => {
  const counter = ref(startCount)
  let timer: number | null = null
  onMounted(() => {
    timer = window.setInterval(() => {
      counter.value++
    }, 1000)
  })
  onUnmounted(() => {
    if (timer) {
      window.clearInterval(timer)
      timer = null
    }
  })

  return readonly(counter)
}

const useIncrement = (startCount = 0) => {
  const counter = ref(startCount)
  const increment = (step = 1) => {
    counter.value += step
  }

  return { counter: readonly(counter), increment }
}
</script>

<script setup lang="ts">
const startCount = 2
const step = 2
const autoCount = useAutoIncrement(startCount)
const { counter, increment } = useIncrement(startCount)
const multiply = computed(() => counter.value * step)
</script>
