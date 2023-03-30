<template>
  <div class="layout">
    <Header />
    <main>
      <!-- <Canvas /> -->
      <slot />
    </main>
  </div>
</template>

<script setup lang="ts">
import Header from '@/components/Header.vue'
// import Canvas from '@/components/three/Canvas.vue'

</script>

<style lang="scss" scoped></style>
