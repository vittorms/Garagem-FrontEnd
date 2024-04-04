<script setup>
import { ref, reactive, onMounted } from "vue";
import AcessorioApi from "@/api/acessorios.js";
const acessorioApi = new AcessorioApi();

const defaultAcessorio = { id: null, descricao: ""};
const acessorios = ref([]);
const acessorio = reactive({ ...defaultAcessorio });

onMounted(async () => {
  acessorios.value = await acessorioApi.buscarTodasAsAcessorio();
  console.log(acessorios.value);
});

function limpar() {
  Object.assign(acessorio, { ...defaultAcessorio });
}

async function salvar() {
  if (acessorio.id) {
    await acessorioApi.atualizarAcessorio(acessorio);
  } else {
    await acessorioApi.adicionarAcessorio(acessorio);
  }
  acessorios.value = await acessorioApi.buscarTodasAsAcessorio();
  limpar();
}

function editar(acessorio_para_editar) {
  Object.assign(acessorio, acessorio_para_editar);
}

async function excluir(id) {
  await acessorioApi.excluirAcessorio(id);
  acessorios.value = await acessorioApi.buscarTodasAsAcessorio();
  limpar();
}
</script>

<template>
  <h1>Acessorio</h1>
  <hr />
  <div class="form">
    <input type="text" v-model="acessorio.descricao" placeholder="Descricao" />
    <button @click="salvar" class="gap">Salvar</button>
    <button @click="limpar">Limpar</button>
  </div>
  <hr />
  <ul>
      <li v-for="acessorio in acessorios" :key="acessorio.id">
        <span @click="editar(acessorio)">
        ({{ acessorio.id }}) - {{ acessorio.descricao }}
      </span>
      <button @click="excluir(acessorio.id)">X</button>
    </li>
  </ul>
</template>