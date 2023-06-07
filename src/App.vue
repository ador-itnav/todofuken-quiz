<script setup lang="ts">
import { ref, computed } from 'vue';
import { PREFECTURES } from './prefectures';

interface Answer {
  prefecture: string;
  city: string;
  checked1: boolean | null;
  checked2: boolean | null;
}

const answers = ref<Answer[]>(
  PREFECTURES.map(() => ({
    prefecture: '',
    city: '',
    checked1: null,
    checked2: null,
  }))
);

const checkAnswers = () => {
  for (let i = 0; i < answers.value.length; i++) {
    const answer = answers.value[i];
    const prefecture = PREFECTURES[i];
    answer.checked1 = answer.prefecture === prefecture.name;
    answer.checked2 = answer.city === prefecture.city;
  }
};

const resetAnswer = (index: number, field: 'checked1' | 'checked2') => {
  answers.value[index][field] = null;
};

const resetAnswers = () => {
  for (let i = 0; i < answers.value.length; i++) {
    resetAnswer(i, 'checked1');
    resetAnswer(i, 'checked2');
  }
};

const getCorrectCount = computed(() => {
  let prefectureCorrectCount = 0;
  let cityCorrectCount = 0;
  for (const answer of answers.value) {
    if (answer.checked1 === true) {
      prefectureCorrectCount++;
    }
    if (answer.checked2 === true) {
      cityCorrectCount++;
    }
  }
  return prefectureCorrectCount + cityCorrectCount;
});

const enableInput = (index: number, field: 'checked1' | 'checked2') => {
  answers.value[index][field] = null;
};
</script>

<template>
  <v-header>
    <h1 class="title">都道府県クイズ</h1>
  </v-header>
  <h2 class="">都道府県一覧</h2>
  <img
    class="map"
    src="https://upload.wikimedia.org/wikipedia/commons/5/57/Japan_prefectures-ja.png"
  />

  <main class="main">
    <div>
      <!-- <h2>都道府県一覧</h2>
      <img
        class="map"
        src="https://upload.wikimedia.org/wikipedia/commons/5/57/Japan_prefectures-ja.png"
      /> -->

      <v-table class="table">
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
          <tr
            v-for="(prefecture, index) in PREFECTURES"
            :key="prefecture.number"
          >
            <td>{{ prefecture.number }}</td>
            <td>
              <v-text-field
                v-model="answers[index].prefecture"
                :disabled="answers[index].checked1 !== null"
              />
            </td>
            <td>
              <v-text-field
                v-model="answers[index].city"
                :disabled="answers[index].checked2 !== null"
              />
            </td>
            <td>
              <template v-if="answers[index].checked1 !== null">
                <span v-if="answers[index].checked1">{{
                  answers[index].checked1 ? '○' : '×'
                }}</span>
                <span v-else>
                  <v-btn @click="() => resetAnswer(index, 'checked1')">
                    ×
                  </v-btn>
                </span>
              </template>
            </td>
            <td>
              <template v-if="answers[index].checked2 !== null">
                <span v-if="answers[index].checked2">{{
                  answers[index].checked2 ? '○' : '×'
                }}</span>
                <span v-else>
                  <v-btn @click="() => resetAnswer(index, 'checked2')">
                    ×
                  </v-btn>
                </span>
              </template>
            </td>
          </tr>
        </tbody>
      </v-table>
    </div>
  </main>

  <p></p>

  <footer class="footer">
    <v-btn @click="checkAnswers">正誤判定</v-btn>
    <p>正解数: {{ getCorrectCount }} / {{ PREFECTURES.length * 2 }}</p>
  </footer>
  <space></space>
  <space> </space>
</template>

<style scoped>
.title {
  margin-bottom: 8px;
  text-align: center;
}

h2 {
  padding-left: 250px;
}

.main {
  display: flex;
  flex-direction: column;
  height: auto;
  /* align-items: center; */
}

.table {
  max-width: 700px;
  border-radius: 30px;
  border: solid 1px;
  padding: 30px;
  margin-left: auto;
  margin-right: auto;
}
.map {
  align-items: center;
  max-width: 600px;
  max-height: 600px;
  float: left;
  margin: auto;
  margin-left: 50px;
  border: 1px solid black;
  position: sticky;
}
.footer {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
