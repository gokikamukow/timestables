<script setup>
import { ref, onMounted } from 'vue'
import { useStorage } from '@vueuse/core'
import Tester from './components/Tester.vue';

const min = useStorage('min', 1);
const max = useStorage('max', 12);
const trials = useStorage('trials', 20);

const openDrawer = ref(false);

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
  <w-button
    @click="openDrawer = true"
  >
    Setup
  </w-button>

  <Tester
  :min="min"
  :max="max"
  :trials="trials"/>

  <w-drawer
    v-model="openDrawer"
    bottom
    height="100px"
  >
    <w-flex column>
      <w-flex justify-start>
        <w-input
          v-model="min"
          label="Min"
          label-position="left"
          type="number"/>
        <w-input
          label="Max"
          label-position="left"
          v-model="max"
          type="number"/>
        <w-input
          label="Trials"
          label-position="left"
          v-model="trials"
          type="number"/>
      </w-flex>
    </w-flex>
  </w-drawer>
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
