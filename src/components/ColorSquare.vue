<script setup lang="ts">
import { computed } from 'vue';

const props = defineProps<{
  color: Color
}>()

const isDarkColor = computed(() => {
    const hex = props.color.hex.replace(/^#/, '');
    const r = parseInt(hex.substring(0, 2), 16);
    const g = parseInt(hex.substring(2, 4), 16);
    const b = parseInt(hex.substring(4, 6), 16);
    const luminance = (0.299 * r + 0.587 * g + 0.114 * b) / 255;
    return luminance < 0.5;
})

</script>

<template>
  <article class="w-64 h-32 p-2 text-3xl font-light" :style="{backgroundColor: color.hex}" :class="{'text-white/60': isDarkColor, 'text-black/60': !isDarkColor}">
    {{ color.name }}
  </article>
</template>
