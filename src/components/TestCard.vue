<script setup>
import { ref, defineProps, computed } from "vue";

const props = defineProps({
  word: {
    type: Object,
    default: () => {},
  },
});

const showWord = computed(() => {
  return isActive.value ? props.word.test : props.word.answer;
});
const isActive = ref(false);

const changeWord = () => {
  isActive.value = !isActive.value;
};
</script>
<template>
  <v-card
    height="100vh"
    class="d-flex justify-center align-center"
    @click="changeWord()"
  >
    <span class="text-h2">{{ showWord }} </span>
  </v-card>
</template>