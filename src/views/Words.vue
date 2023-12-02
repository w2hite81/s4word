<script setup>
import TestCard from "@/components/TestCard.vue";
import datas from "@/datas/works.json";
import { ref, computed } from "vue";

const items = ref(datas);

const selected = ref([]);

const randomWord = (a) => {
  let arr = [...a];
  const random = Math.floor(Math.random() * arr.length);
  let test = arr[random];
  arr.splice(random, 1);

  return { test, answer: arr[0], isActive: false };
};

const randomWords = computed(() => {
  let temps = [];

  selected.value.forEach((item) => {
    temps.push(randomWord([item.word, item.trans]));
  });

  return temps;
});

const modal = ref(false);

const openTest = () => {
  if (selected.value.length == 0) return alert("선택해주세요.");
  modal.value = true;
};
</script>
<template>
  <v-card
    full-height
    class="d-flex align-center"
    image="@/assets/images/po.jpeg"
  >
    <div class="mx-auto">
      <v-card width="350px" title="단어장" color="primary" class="mb-2">
        <v-card-text>
          <v-list
            v-model:selected="selected"
            select-strategy="classic"
            class="pa-0"
            height="calc(100vh - 220px)"
          >
            <template v-for="(item, i) in items" :key="i">
              <v-list-item border :value="item">
                <template v-slot:prepend="{ isActive }">
                  <v-list-item-action start>
                    <v-checkbox-btn :model-value="isActive"></v-checkbox-btn>
                  </v-list-item-action>
                </template>
                <v-list-item-title class="d-flex justify-space-between">
                  <span>{{ item.word }}</span
                  ><span>{{ item.trans }}</span>
                </v-list-item-title>
              </v-list-item>
            </template>
          </v-list>
        </v-card-text>
        <v-card-actions class="pa-4">
          <v-btn @click="openTest" width="100%" variant="flat">TEST</v-btn>

          <v-dialog fullscreen v-model="modal">
            <template v-slot:default="{ isActive }">
              <v-card>
                <v-card-title class="d-flex justify-end">
                  <v-icon @click="isActive.value = false">mdi-close</v-icon>
                </v-card-title>

                <v-window show-arrows>
                  <v-window-item v-for="(word, n) in randomWords" :key="n">
                    <test-card :word="word" />
                  </v-window-item>
                </v-window>
              </v-card>
            </template>
          </v-dialog>
        </v-card-actions>
      </v-card>
      <v-file-input label="File input" density="compact"></v-file-input>
    </div>
  </v-card>
</template>