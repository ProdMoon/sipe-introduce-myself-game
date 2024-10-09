<script setup>

import { ref, watch, onMounted } from 'vue';
const props = defineProps({
  src: String,
});

const audio = ref(null);

watch(
  () => props.src,
  (newVal) => {
    audio.value.src = newVal;
    audio.value.play();
  }
);

onMounted(() => {
  audio.value.addEventListener('ended', () => {
    audio.value.currentTime = 0;
    audio.value.play();
  });
});

const playMusic = () => {
  audio.value.play();
};

const muteMusic = () => {
  audio.value.muted = !audio.value.muted;
};
</script>

<template>
  <div class="music-player">
    <audio ref="audio" controls>
      <source :src="props.src" type="audio/mpeg" />
      Your browser does not support the audio element.
    </audio>
  </div>
</template>