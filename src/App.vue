<script setup>
import { ref, onMounted } from 'vue'
import Tester from './components/Tester.vue';

const min = ref(1);
const max = ref(12);
const trials = ref(20);

onMounted(() => {
  const search_params = new URLSearchParams(window.location.search)
  if (search_params.has('min')) {
    min.value = Number(search_params.get('min'))
  }
  if (search_params.has('max')) {
    max.value = Number(search_params.get('max'))
  }
  if (search_params.has('trials')) {
    trials.value = Number(search_params.get('trials'))
  }
})
</script>

<template>
  <Tester
  :min="min"
  :max="max"
  :trials="trials"/>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
