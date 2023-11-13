<template>
  <div class="about">
    <label for="dataHora">Data: </label>
    <input id="dataHora" type="datetime-local" v-model="dataHora" /> <br />
    <label for="ano">Ano: </label>
    <input id="ano" type="number" v-model="ano" /> <br />
    <label for="semestre">Semestre: </label>
    <input id="semestre" type="text" v-model="semestre" /> <br />
    <label for="tamanho">Tamanho: </label>
    <input id="tamanho" type="text" v-model="tamanho" /> <br />
    <button @click="cadastrar">Cadastrar</button>
    <table>
      <thead>
        <td>Id</td>
        <td>DataHora</td>
        <td>Ano</td>
        <td>Semestre</td>
        <td>Tamanho</td>
      </thead>
      <tr v-for="turma in turmas" :key="turma.id">
        <td>{{ turma.id }}</td>
        <td>{{ turma.dataHoraCadastro }}</td>
        <td>{{ turma.ano }}</td>
        <td>{{ turma.semestre }}</td>
        <td>{{ turma.tamanho }}</td>
      </tr>
    </table>
    <p>{{ vazio }}</p>
  </div>

  <br />

  <div>
    <label for="tamanho2">Tamanho: </label>
    <input id="tamanho2" type="text" v-model="tamanho2" /> <br />
    <label for="ano2">Ano: </label>
    <input id="ano2" type="number" v-model="ano2" /> <br />
    <button @click="buscar">Buscar</button>
  </div>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue'
import axios from 'axios'
const dataHora = ref()
const ano = ref()
const ano2 = ref()
const semestre = ref()
const tamanho = ref()
const tamanho2 = ref()
const turmas = ref()
const vazio = ref()
async function atualizar() {
  turmas.value = (
    await axios.get('https://8080-mineda-springboot3app-ea78m1m7mhp.ws-us106.gitpod.io/turma')
  ).data
}

async function cadastrar() {
  await axios.post('https://8080-mineda-springboot3app-ea78m1m7mhp.ws-us106.gitpod.io/turma', {
    dataHoraCadastro: dataHora.value,
    ano: ano.value,
    semestre: semestre.value,
    tamanho: tamanho.value
  })
  dataHora.value = null
  ano.value = null
  semestre.value = null
  tamanho.value = null
  atualizar()
}

async function buscar() {
  await axios
    .get(
      `https://8080-mineda-springboot3app-ea78m1m7mhp.ws-us106.gitpod.io/turma/${tamanho2.value}/${ano2.value}`
    )
    .then(function (response) {
      if (response.data < 1) {
        vazio.value = 'Nenhuma turma encontrada'
        turmas.value = []
      } else {
        turmas.value = response.data
        vazio.value = null
      }
    })
    .catch(function (error) {
      console.log(error)
    })
}

onMounted(() => {
  atualizar()
})
</script>
