<script setup lang="ts">
defineProps<{
  color: Color
}>()
</script>

<template>
  <article class="w-24 h-24 p-2" :style="{backgroundColor: color.hex}">
    {{ color.name }}
  </article>
</template>
