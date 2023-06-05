<script setup lang="ts">
import { ref } from 'vue';
const primitive = 8;
// 見た目（テンプレート）で使うものは ref で囲む
// 使わないものは囲まない
const reactive = ref(8);

const count = ref(0);

function countUp(num: number) {
  // count.value = count.value + num;
  count.value += num;
  // 変更後の値を戻り値として返す
  return count.value;
}
// 10　が代入される
const updatedCount = countUp(10);

const updatedCount2 = countUp(10);

function createInitial(firstName: string, lastName: string) {
  const firstInitial = firstName[0];
  const lastInitial = lastName[0];

  return firstInitial + '.' + lastInitial;
}

// "t.y"が代入される
primitive; // <- 8

reactive.value; // <-8
</script>

<template>
  <!-- テンプレートの中では、.value が省略されるので注意！ -->
  <div class="label">count: {{ count }}</div>

  <button @click="countUp(10)">Count up</button>

  <main class="main">
    <div class="box">box1</div>
    <div class="box">box2</div>
    <div class="box">box3</div>
  </main>
</template>

<style scoped>
.label {
  margin-bottom: 8px;
}
</style>
