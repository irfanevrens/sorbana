<script setup>

import { ref } from "@vue/reactivity";
import { onMounted } from "@vue/runtime-core";
import { useRoute, useRouter } from "vue-router";

const route = useRoute()
const router = useRouter()

const questionCount = 2;

const teams = ref([
  {
    name: 'ENDÜSTRİ MESLEK LİSESİ',
    point: 0,
    code: 'a',
  },
  {
    name: 'İMAM HATİP LİSESİ',
    point: 0,
    code: 'b',
  },
  {
    name: 'FEN LİSESİ',
    point: 0,
    code: 'c',
  },
  {
    name: 'İMAM HATİP SIBYAN MEKTEBİ',
    point: 0,
    code: 'd',
  },
])

const getTeamPoint = (index) => {

  const key = 'team_point_' + index

  const actualPoint = localStorage.getItem(key) ?? 0

  return parseInt(actualPoint)

}

const persistTeamPoint = (index, value) => {

  const key = 'team_point_' + index

  localStorage.setItem(key, value)

}

const infoShowing = ref(true)
const questionShowing = ref(false)
const answerShowing = ref(false)

const plusTeamPoint = (index) => {

  if (teams.value[index].point >= 0) {

    teams.value[index].point++

    persistTeamPoint(index, teams.value[index].point)

  }

}

const minusTeamPoint = (index) => {

  if (teams.value[index].point >= 1) {
    teams.value[index].point--

    persistTeamPoint(index, teams.value[index].point)


  }

}

const gotoNextPage = () => {

  infoShowing.value = true
  questionShowing.value = false
  answerShowing.value = false

  const activePage = parseInt(route.params.index);

  if (activePage < questionCount) {

    router.push({
      name: 'soru',
      params: {
        index: activePage + 1,
      }
    })

  }

}

const gotoPrevPage = () => {

  infoShowing.value = true
  questionShowing.value = false
  answerShowing.value = false
  
  const activePage = parseInt(route.params.index);

  if (activePage > 1) {

    router.push({
      name: 'soru',
      params: {
        index: activePage - 1,
      }
    })

  }

}

onMounted(() => {

  teams.value.forEach((team, index) => {

    teams.value[index].point = getTeamPoint(index)

  })

  window.onkeyup = function (event) {

    const tappedKey = event.key.toUpperCase()

    console.log(tappedKey)

    switch (tappedKey) {

      case 'A':
        plusTeamPoint(0)
        break;

      case 'S':
        minusTeamPoint(0)
        break;

      case 'B':
        plusTeamPoint(1)
        break;

      case 'N':
        minusTeamPoint(1)
        break;

      case 'C':
        plusTeamPoint(2)
        break;

      case 'V':
        minusTeamPoint(2)
        break;

      case 'D':
        plusTeamPoint(3)
        break;

      case 'F':
        minusTeamPoint(3)
        break;

      case 'ARROWLEFT':

        gotoPrevPage()

        break;

      case 'ARROWRIGHT':

        gotoNextPage()

        break;

      case 'ARROWUP':

        if (answerShowing.value) {

          answerShowing.value = false

        } else {

          if (questionShowing.value) {

            questionShowing.value = false
            infoShowing.value = true

            return

          }

        }

        break;

      case 'ARROWDOWN':

        if (infoShowing.value) {

          infoShowing.value = false
          questionShowing.value = true

        } else {

          if (questionShowing.value) {

            answerShowing.value = true

          }

        }

        break;

    }

  }

})

</script>

<template>

  <div>

    <div class="soru-kutusu">

      <div v-if="infoShowing">
        <img :src="`/infos/${route.params.index}.jpeg`" width="300">
      </div>

      <div v-if="questionShowing">
        <img :src="`/questions/${route.params.index}.jpeg`" width="300">
      </div>

    </div>
    <div class="cevap-kutusu">

      <div v-if="answerShowing">
        <img :src="`/answers/${route.params.index}.jpeg`" width="300">
      </div>

    </div>
    <div class="araba-kutusu">

      <table>

        <tbody>
        <tr v-for="team in teams">
          <td>{{ team.name }}</td>
          <td>
            <img :style="{left: `${team.point * 50}px`}" :src="`/cars/${team.code}.jpeg`" width="100">
          </td>
        </tr>
        </tbody>

      </table>

    </div>

  </div>

</template>


<style scoped>

.soru-kutusu {
  border-left: thin solid #32a1ce;
  border-top: thin solid #32a1ce;
  border-right: thin solid #32a1ce;
  height: 45vh;
  padding: 5px;
  text-align: center;
}

.cevap-kutusu {
  border-left: thin solid #32a1ce;
  border-top: thin solid #32a1ce;
  border-right: thin solid #32a1ce;
  height: 25vh;
  padding: 5px;
  text-align: center;
}

.araba-kutusu {
  border-left: thin solid #32a1ce;
  border-top: thin solid #32a1ce;
  border-right: thin solid #32a1ce;
  border-bottom: thin solid #32a1ce;
  height: 30vh;
  padding: 5px;
}

</style>