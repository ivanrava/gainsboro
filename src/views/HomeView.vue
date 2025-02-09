<script setup lang="ts">
import ColorSquare from '@/components/ColorSquare.vue';
import { colornames } from 'color-name-list';
import { computed, ref } from 'vue';

function hexToHSL(hex: string) {
    hex = hex.replace(/^#/, '');
    const r = parseInt(hex.substring(0, 2), 16) / 255;
    const g = parseInt(hex.substring(2, 4), 16) / 255;
    const b = parseInt(hex.substring(4, 6), 16) / 255;

    const max = Math.max(r, g, b), min = Math.min(r, g, b);
    let h, s = (max + min) / 2;
    const l = (max + min) / 2;

    if (max === min) {
        h = s = 0; // scala di grigi
    } else {
        const d = max - min;
        s = l > 0.5 ? d / (2 - max - min) : d / (max + min);
        switch (max) {
            case r: h = (g - b) / d + (g < b ? 6 : 0); break;
            case g: h = (b - r) / d + 2; break;
            case b: h = (r - g) / d + 4; break;
        }
        h *= 60;
    }
    return { h, s, l };
}

const filter = ref<string>("")

const filteredList = computed(() => {
  return colornames
    .filter((c: Color) => c.name.toLowerCase().includes(filter.value.toLowerCase()))
    .sort((a: Color, b: Color) => hexToHSL(a.hex).h! - hexToHSL(b.hex).h!)
})

const savedColors = ref<Color[]>([])
</script>

<template>
  <main>
    <input class="w-full h-12 p-4 text-3xl pb-5 focus:outline-0 sticky top-0 bg-slate-300 dark:bg-stone-900 dark:text-white/90 shadow-xl" placeholder="Filter" v-model="filter" />
    <div class="flex flex-wrap justify-center">
      <ColorSquare v-for="color in filteredList" :color="color" :key="color.name" @click="savedColors.push(color)" class="cursor-pointer" />
    </div>
    <aside class="bg-slate-300 p-4 fixed bottom-0 w-full shadow-xl dark:bg-stone-900">
      <div class="flex flex-wrap">
        <ColorSquare v-for="color in savedColors" :color="color" :key="color.name" />
      </div>
    </aside>
  </main>
</template>
