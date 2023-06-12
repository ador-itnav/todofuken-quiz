<script setup lang="ts">
import { ref, computed } from 'vue';
import { PREFECTURES } from './prefectures';

interface Answer {
  prefecture: string;
  city: string;
  isCorrectPrefecture: boolean | null;
  isCorrectCity: boolean | null;
}

const answers = ref<Answer[]>(
  PREFECTURES.map(() => ({
    prefecture: '',
    city: '',
    isCorrectPrefecture: null,
    isCorrectCity: null,
  }))
);

const checkAnswers = () => {
  for (let i = 0; i < answers.value.length; i++) {
    const answer = answers.value[i];
    const prefecture = PREFECTURES[i];
    answer.isCorrectPrefecture = answer.prefecture === prefecture.name;
    answer.isCorrectCity = answer.city === prefecture.city;
  }
};

const resetAnswer = (
  index: number,
  field: 'isCorrectPrefecture' | 'isCorrectCity'
) => {
  answers.value[index][field] = null;
};

const resetAnswers = () => {
  for (let i = 0; i < answers.value.length; i++) {
    resetAnswer(i, 'isCorrectPrefecture');
    resetAnswer(i, 'isCorrectCity');
  }
};

const getCorrectCount = computed(() => {
  let prefectureCorrectCount = 0;
  let cityCorrectCount = 0;
  for (const answer of answers.value) {
    if (answer.isCorrectPrefecture === true) {
      prefectureCorrectCount++;
    }
    if (answer.isCorrectCity === true) {
      cityCorrectCount++;
    }
  }
  return prefectureCorrectCount + cityCorrectCount;
});

const enableInput = (
  index: number,
  field: 'isCorrectPrefecture' | 'isCorrectCity'
) => {
  answers.value[index][field] = null;
};
</script>

<template>
  <div class="app" id="app">
    <router-view />
    <header>
      <h1 class="title">都道府県クイズ</h1>
    </header>
    <h2 class="">都道府県一覧</h2>
    <img
      class="map"
      src="https://upload.wikimedia.org/wikipedia/commons/5/57/Japan_prefectures-ja.png"
    />

    <main class="main">
      <div>
        <v-table density="compact" height="900px" class="table">
          <thead>
            <tr>
              <th class="table-header">番号</th>
              <th class="table-header">都道府県名</th>
              <th class="table-header">県庁所在地</th>
              <th class="table-header">正誤欄1</th>
              <th class="table-header">正誤欄2</th>
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
                  variant="underlined"
                  density="compact"
                  v-model="answers[index].prefecture"
                  :disabled="answers[index].isCorrectPrefecture !== null"
                />
              </td>
              <td>
                <v-text-field
                  variant="underlined"
                  density="compact"
                  v-model="answers[index].city"
                  :disabled="answers[index].isCorrectCity !== null"
                />
              </td>
              <td>
                <template v-if="answers[index].isCorrectPrefecture !== null">
                  <span v-if="answers[index].isCorrectPrefecture">{{
                    answers[index].isCorrectPrefecture ? '○' : '×'
                  }}</span>
                  <span v-else>
                    <v-btn
                      density="compact"
                      @click="() => resetAnswer(index, 'isCorrectPrefecture')"
                    >
                      ×
                    </v-btn>
                  </span>
                </template>
              </td>
              <td>
                <template v-if="answers[index].isCorrectCity !== null">
                  <span v-if="answers[index].isCorrectCity">{{
                    answers[index].isCorrectCity ? '○' : '×'
                  }}</span>
                  <span v-else>
                    <v-btn
                      density="compact"
                      @click="() => resetAnswer(index, 'isCorrectCity')"
                    >
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

    <footer class="footer">
      <v-btn density="compact" @click="checkAnswers">正誤判定</v-btn>
      <p>正解数: {{ getCorrectCount }} / {{ PREFECTURES.length * 2 }}</p>
    </footer>
    <space></space>
    <space> </space>
  </div>
</template>

<style scoped>
@import './style.css';
@import url('https://fonts.googleapis.com/css2?family=Yuji+Mai&display=swap');
.app {
  font-family: 'Yuji Mai', serif;
}
.title {
  font-size: 70px;
  margin-bottom: 8px;
  text-align: center;
  filter: drop-shadow(10px 10px 8px black);
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
  line-height: 30px;
}
.table-header {
  text-align: center;
}
.map {
  align-items: center;
  max-width: 600px;
  max-height: 600px;
  float: left;
  margin: auto;
  margin-left: 50px;
  border: 1px solid white;
  position: sticky;
  top: 60%;
  transform: translateY(-50%);
}
.footer {
  padding-top: 30px;
  padding-bottom: 30px;
  padding-left: 600px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

@media (max-width: 740px) {
  h2 {
    padding-left: 0;
  }

  .main {
    align-items: flex-start;
  }

  .table {
    max-width: 100%;
    margin-left: 20px;
    margin-right: 20px;
  }

  .footer {
    padding-left: 0;
  }
}
</style>
