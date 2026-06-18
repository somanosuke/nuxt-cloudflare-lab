<script setup lang="ts">
import { ref } from "vue";

interface Block {
  id: string;
  height: number;
  width: number;
}

const formWidth = ref<number>();
const formHeight = ref<number>();

const blocks = ref<Block[]>([
  {
    id: crypto.randomUUID(),
    height: 100,
    width: 300,
  },
  {
    id: crypto.randomUUID(),
    height: 166,
    width: 500,
  },
  {
    id: crypto.randomUUID(),
    height: 250,
    width: 400,
  },
]);

function addBlock() {
  if (!formWidth.value || !formHeight.value) return;

  blocks.value.push({
    id: crypto.randomUUID(),
    height: formHeight.value,
    width: formWidth.value,
  });

  formWidth.value = undefined;
  formHeight.value = undefined;
}
</script>

<template>
  <div class="ad-tester">
    <div class="blocks-wrapper">
      <TransitionGroup name="block-fade">
        <div
          v-for="block in blocks"
          :key="block.id"
          class="block"
          :style="{ height: block.height + 'px', width: block.width + 'px' }"
        >
          <p class="size-text">横:{{ block.width }}px 縦:{{ block.height }}px</p>
        </div>
      </TransitionGroup>
    </div>
    <div class="add-block-form">
      <input type="text" name="width" placeholder="横の長さ" v-model="formWidth" />
      <input type="text" name="height" placeholder="縦の長さ" v-model="formHeight" />
      <button @click="addBlock()">追加</button>
    </div>
  </div>
</template>
<style>
.blocks-wrapper {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}
.block {
  background-color: rgb(51, 51, 51);
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 2px 2px 4px rgb(170, 170, 170);
  transition: 0.2s;
}
.block:hover {
  cursor: pointer;
  transform: translate3D(-2px, -2px, 0px);
  transition: 0.2s;
}
.size-text {
  color: white;
}

.block-fade-enter-active,
.block-fade-leave-actice {
  transform: translateX(0);
  transition: 0.3s;
}
.block-fade-enter-from,
.block-fade-leave-to {
  opacity: 0;
  transform: translateX(30px);
  transition: 0.3s;
}
</style>
