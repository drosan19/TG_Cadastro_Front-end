<template>

  <div class="cadastro">

    <label for="aluno">Aluno: </label>
    <input id="aluno" type="text" v-model="aluno" /><br>
    <br>

    <label for="orientador">Orientador: </label>
    <input id="orientador" type="text" v-model="orientador" /><br>
    <br>
    
    <label for="dataHoraApresentacao">Data Hora Apresentacao: </label>
    <input id="dataHoraApresentacao" type="datetime-local" v-model="dataHoraApresentacao" /><br>
    <br>

    <button @click="cadastrar">Cadastrar</button><br>
    <br>
  
    <table>
      <thead>
        <td>Id</td>
        <td>Aluno</td>
        <td>Nota</td>
        <td>Orientador</td>
        <td>Data_Hora_Apresentacao</td>

      </thead>
      <tr v-for="cadastro in cadastros" :key="cadastro.id">
        <td>{{ cadastro.id }}</td>
        <td>{{ cadastro.aluno }}</td>
        <td>{{ cadastro.nota }}</td>
        <td>{{ cadastro.orientador }}</td>
        <td>{{ cadastro.dataHoraApresentacao }}</td>
      </tr>
    </table>

    <br>
    <h2>Atribuir Nota TG</h2><br>
    <label>Selecine o ID: </label>

    <select id="cadastroSelect" v-model="selectedId">
      <option v-for="cadastro in cadastros" :key="cadastro.id" :value="cadastro.id">
        {{ cadastro.id }}
      </option>
    </select> 
    
    <br><br>
    <label for="nota">Nota: </label>
    <input id="nota" type="number" v-model="nota" /><br>
    
    <br>
    <button @click="alterarNota">Cadastrar Nota</button><br>

  </div>
</template>

<style>
  table, th, td {
    border:1px solid black;
  }
</style>

<script setup lang="ts">
import { onMounted, ref } from 'vue';
import axios from 'axios';

const aluno = ref("");
const orientador = ref("");
const nota = ref("");
const dataHoraApresentacao = ref("");
const selectedId = ref("")

const cadastros = ref();
async function atualizar() {
 cadastros.value = (await axios.get("https://8080-shaka20100-springboot3l-l56ki85u9c5.ws-us115.gitpod.io/cadastro")).data;
}

async function cadastrar() {
 await axios.post("https://8080-shaka20100-springboot3l-l56ki85u9c5.ws-us115.gitpod.io/cadastro",
 {
    aluno: aluno.value,
    orientador: orientador.value,
    dataHoraApresentacao: dataHoraApresentacao.value
 });
    aluno.value = "";
    orientador.value = "";
    dataHoraApresentacao.value = "";
 atualizar();
}

async function alterarNota() {
  await axios.put(`https://8080-shaka20100-springboot3l-l56ki85u9c5.ws-us115.gitpod.io/cadastro/${selectedId.value}/nota?novaNota=${nota.value}`);
 selectedId.value = "";
 nota.value = "";
  atualizar();
}


onMounted(() => {
 atualizar();
});
</script>
