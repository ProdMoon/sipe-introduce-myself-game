<script setup>
import { defineProps, onMounted, ref, watch } from 'vue';

const props = defineProps({
  content: String,
  requestNext: Function,
});

const displayContent = ref('');
const timeout = ref(null);
const readyToNext = ref(false);

const typewriter = async (text) => {
  let i = 0;
  const speed = 50;
  displayContent.value = '';
  return new Promise((resolve) => {
    const type = () => {
      if (i < text.length) {
        displayContent.value += text.charAt(i);
        i++;
        timeout.value = setTimeout(type, speed);
      } else {
        resolve();
      }
    };
    type();
  });
};

const skipTypewriter = () => {
  clearTimeout(timeout.value);
  displayContent.value = props.content;
};

const handleAreaClick = () => {
  if (readyToNext.value) {
    readyToNext.value = false;
    props.requestNext();
  } else {
    skipTypewriter();
    setTimeout(() => {
      readyToNext.value = true;
    }, 300);
  }
};

watch(
  () => props.content,
  async (newVal) => {
    await typewriter(newVal);
    setTimeout(() => {
      readyToNext.value = true;
    }, 300);
  }
);

onMounted(async () => {
  await typewriter(props.content);
  setTimeout(() => {
    readyToNext.value = true;
  }, 300);
});
</script>

<template>
  <section
    class="absolute bottom-0 h-1/4 w-full rounded-3xl border-[12px] border-double border-gray-500 bg-white p-5 font-orbit text-lg lg:p-8 lg:text-2xl"
    @click="handleAreaClick"
  >
    {{ displayContent }}
    <div
      v-if="readyToNext"
      class="absolute bottom-1 right-1 animate-bounce rounded-xl bg-black p-2 text-sm text-white"
    >
      <svg
        xmlns="http://www.w3.org/2000/svg"
        viewBox="0 0 24 24"
        fill="currentColor"
        class="size-6"
      >
        <path
          fill-rule="evenodd"
          d="M13.28 11.47a.75.75 0 0 1 0 1.06l-7.5 7.5a.75.75 0 0 1-1.06-1.06L11.69 12 4.72 5.03a.75.75 0 0 1 1.06-1.06l7.5 7.5Z"
          clip-rule="evenodd"
        />
        <path
          fill-rule="evenodd"
          d="M19.28 11.47a.75.75 0 0 1 0 1.06l-7.5 7.5a.75.75 0 1 1-1.06-1.06L17.69 12l-6.97-6.97a.75.75 0 0 1 1.06-1.06l7.5 7.5Z"
          clip-rule="evenodd"
        />
      </svg>
    </div>
  </section>
</template>
