<script setup lang="ts">
import { computed } from "vue";
import { handleBackground } from "../layoutHelper";

const props = defineProps({
  background: {
    // random image from a curated Unsplash collection by Anthony
    default: "https://source.unsplash.com/collection/94734566/1920x1080",
  },
});

const positions = ['20% 20%', '50% 20%', '80% 20%', '20% 50%', '80% 50%', '20% 80%', '50% 80%', '80% 80%'];
const randomPosition = positions[Math.floor(Math.random() * positions.length)];
const randomZoom = `${110 + Math.floor(Math.random() * 41)}%`;

const bgStyle = computed(() => ({
  ...handleBackground(props.background, false),
  backgroundPosition: randomPosition,
  backgroundSize: randomZoom,
  filter: 'blur(3px)',
}));
</script>

<template>
  <div class="slidev-layout cover text-center overflow-hidden">
    <div class="absolute inset-[-10px]" :style="bgStyle"></div>
    <div class="relative my-auto w-full bg-[rgba(28,28,28,0.7)] backdrop-blur-md py-10 rounded-lg">
      <slot />
    </div>
  </div>
</template>
