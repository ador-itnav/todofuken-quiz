<script setup lang="ts">
import { PREFECTURES } from './prefectures';
/*checkAnswer*/
PREFECTURES[0].name;
methods:{
function checkAnswer1() {
if (answeredName === PREFECTURES.name) {
    return '○';
  } else {
    return '×';
  }
}}
};
</script>

<template>
  <header>
    <h1 class="title">都道府県クイズ</h1>
  </header>

  <main class="main">
    <!-- <img
      class="map"
      src="https://upload.wikimedia.org/wikipedia/commons/5/57/Japan_prefectures-ja.png"
    />

    <input type="text" /> -->

    <!--// コンテナ要素を取得 const container =
    document.getElementById('container'); // 0から47までの数値を生成 for (let i
    = 0; i <= 47; i++) { // テキストボックス要素を生成 const textBox =
    document.createElement('input'); textBox.type = 'text'; textBox.value =
    i.toString(); // テキストボックスをコンテナに追加
    container.appendChild(textBox); // 改行を挿入 if ((i + 1) % 4 === 0) {
    container.appendChild(document.createElement('br')); } } -->
    <div>
      <h2>都道府県一覧</h2>
      <table>
        <thead>
          <tr>
            <th>番号</th>
            <th>都道府県名</th>
            <th>県庁所在地</th>
            <th>正誤欄1</th>
            <th>正誤欄2</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="prefecture in PREFECTURES" :key="prefecture.number">
            <td>{{ prefecture.number }}</td>
            <td><input v-model="answeredName" type="text" /></td>
            <td><input v-model="answeredCity" type="text" /></td>
            <td><input type="text" /></td>
            <td>
              <input type="text" @input="checkAnswer1" />
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </main>

  <footer></footer>
</template>

<style scoped>
.title {
  margin-bottom: 8px;
  text-align: center;
}
.main {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.table {
}
.map {
  max-width: 70%;
  max-height: 50%;
}
</style>
