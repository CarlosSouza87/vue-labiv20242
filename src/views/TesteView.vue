<template>
    <div class="about">
      <h1 id="hello">Chamados</h1>
      <form @submit.prevent="novo">
        <p>
          <label for="nome" id="labelTitulo">Título: </label>
          <input id="titulo" type="text" v-model="chamado.titulo" required/>
        </p>
        <p>
          <label for="descricao" id="labelDescricao">Descrição: </label>
          <input id="descricao" type="text" v-model="chamado.descricao"/>
        </p>
        <p>
          <label for="dataHoraCadastro" id="labelDataHoraCadastro">Data/hora Cadastro: </label>
          <input id="dataHoraCadastro" type="datetime-local" v-model="chamado.dataHoraCadastro"/>
        </p>
        <p>
          <label for="dataHoraFinalizacao" id="labelDataHoraFinalizacao">Data/hora Finalização: </label>
          <input id="dataHoraFinalizacao" type="datetime-local" v-model="chamado.dataHoraFinalizacao"/>
        </p>
        <p>
          <label for="estimativa" id="labelEstimativa">Estimativa: </label>
          <input id="estimativa" type="number" step="0.1" v-model="chamado.estimativa"/>
        </p>
        <button type="submit">Incluir</button>
        <p v-if="erro">{{ erro }}</p>
      </form>
      <div id="detalhes" v-if="chamados.length > 0">
        <h2>Detalhes</h2>
        <p>
          <label for="chamadoDetalhe" id="labelChamadoDetalhe">Chamado: </label>
          <select name="chamadoDetalhe" id="chamadoDetalhe" v-model="chamadoDetalhe">
            <option v-for="(cmd, index) in chamados" :key="index" :value="cmd">{{ cmd.titulo }}</option>
          </select>
          <div id="detalhe" v-if="chamadoDetalhe.id">
            <p>Id: {{ chamadoDetalhe.id }}</p>
            <p>Título: {{ chamadoDetalhe.titulo }}</p>
            <p>Descrição: {{ chamadoDetalhe.descricao }}</p>
            <p>Data/hora Cadastro: {{ chamadoDetalhe.dataHoraCadastro }}</p>
            <p>Data/hora Finalização: {{ chamadoDetalhe.dataHoraFinalizacao }}</p>
            <p>Estimativa: {{ chamadoDetalhe.estimativa }}</p>
          </div>
        </p>
      </div>
      <h2>Listagem</h2>
      <table>
        <thead>
          <th>Id</th>
          <th>Título</th>
          <th>Data/hora cadastro</th>
          <th>Data/hora finalização</th>
        </thead>
        <tbody>
          <tr v-for="(cmd, index) in chamados" :key="index">
            <td>{{ cmd.id }}</td>
            <td>{{ cmd.titulo }}</td>
            <td>{{ cmd.dataHoraCadastro }}</td>
            <td>{{ cmd.dataHoraFinalizacao? cmd.dataHoraFinalizacao:'Aberto'  }}</td>
          </tr>
        </tbody>
      </table>
  
    </div>
  </template>
  
  <script setup lang="ts">
  
  import { ref } from 'vue';
  import axios from 'axios';
  import { onMounted } from 'vue';
  
  const chamado = ref(
    {
      titulo: 'chamado',
      descricao: '',
      dataHoraCadastro: new Date(),
      dataHoraFinalizacao: null,
      estimativa: null
    }
  );
  const chamadoDetalhe = ref(
    {
      id: null,
      titulo: 'chamado',
      descricao: '',
      dataHoraCadastro: new Date(),
      dataHoraFinalizacao: null,
      estimativa: null
    }
  );
  const erro = ref<string>();
  
  const chamados = ref([{ id: 1, titulo: "teste", descricao: "", dataHoraCadastro: new Date(), dataHoraFinalizacao: new Date(), estimativa: 2.0 }]);
  

  async function todos() {
    try {
        chamados.value = (await axios.get('chamado')).data;
        erro.value = '';
    }
    catch(ex) {
        erro.value = (ex as Error).message;
    }
  }

  async function novo() {
    try {
        await axios.post('chamado', chamado.value );
        todos();
        erro.value = '';
    }
    catch(ex) {
        erro.value = (ex as Error).message;
    }
  }

  onMounted(() => {
    todos();
  });
  
  </script>