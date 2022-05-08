<script setup>

import { ref } from "@vue/reactivity";
import { onMounted } from "@vue/runtime-core";
import { useRoute } from "vue-router";

const route = useRoute()

const teams = ref([
  {
    name: 'A Takımı',
    point: 0,
    code: 'a',
  },
  {
    name: 'B Takımı',
    point: 0,
    code: 'b',
  },
  {
    name: 'C Takımı',
    point: 0,
    code: 'c',
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

const showQuestion = ref(false)
const showAnswer = ref(false)

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

onMounted(() => {

  teams.value.forEach((team, index) => {

    teams.value[index].point = getTeamPoint(index)

  })

  window.onkeyup = function (event) {

    const tappedKey = event.key.toUpperCase()

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


        break;

      case 'V':


        break;


      case 'D':


        break;

      case 'F':


        break;

    }

  }

})

</script>

<template>
  <table width="100%">
    <tbody>
    <tr>
      <td height="400">

        <main style="text-align: center">

          <div v-if="! showQuestion">
            <img :src="`/infos/${route.params.index}.jpeg`" width="300">
          </div>

          <p v-if="! showQuestion">
            <button @click="showQuestion = true">Soruyu Göster</button>
          </p>

          <table>
            <tbody>
            <tr>
              <td height="300">
                <div v-if="showQuestion">
                  <img :src="`/questions/${route.params.index}.jpeg`" width="300">
                </div>
              </td>
            </tr>
            <tr>
              <td>
                <p v-if="! showAnswer && showQuestion">
                  <button @click="showAnswer = true">Cevabı Göster</button>
                </p>
              </td>
            </tr>
            <tr>
              <td height="100">
                <div v-if="showAnswer && showQuestion">
                  <img :src="`/answers/${route.params.index}.jpeg`" width="300">
                </div>
              </td>
            </tr>
            </tbody>
          </table>

        </main>

      </td>
      <td>

        <ul>
          <li><a href="/soru/1">Soru 1</a></li>
          <li><a href="/soru/2">Soru 2</a></li>
          <li><a href="/soru/3">Soru 3</a></li>
        </ul>

      </td>
    </tr>
    <tr>
      <td colspan="2">

        <table>

          <tbody>
          <tr v-for="team in teams">
            <td>{{ team.name }}</td>
            <td>
              <img :style="{left: `${team.point * 50}px`}" :src="`/cars/${team.code}.jpeg`" width="200">
            </td>
          </tr>
          </tbody>

        </table>

      </td>
    </tr>
    </tbody>
  </table>
</template>
