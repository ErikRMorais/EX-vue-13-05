<template>
  <h1>Cadastro de alunos</h1>
  <button @click="mostraForm = true">Adicionar Aluno</button>
  <div v-if="mostraForm">
    <input v-model="nome" placeholder="Nome">
    <input v-model="matricula" placeholder="Matrícula">
    <input v-model="curso" placeholder="Curso">
    <input v-model="ativo" type="checkbox">
    <button @click="salvar">Salvar</button>
    <button @click="limpar">Cancelar</button>
  </div>
  <ul>
    <li v-for="(aluno, index) in alunos" :key="index">
      {{ aluno.nome }} - {{ aluno.matricula }} - {{ aluno.curso }} - {{ aluno.ativo }}
      <button @click="editar(aluno)">Editar</button>
      <button @click="excluir(aluno.id)">Excluir</button>
    </li>
  </ul>

</template>

<script setup>
import { ref } from 'vue'

const alunos = ref([]);
const mostraForm = ref(false);
const edit = ref(null);

const nome =ref('')
const matricula= ref('')
const curso = ref('')
const ativo = ref(true);

function salvar(){
  if(edit.value===null){
    alunos.value.push({id: Date.now(),nome:nome.value,matricula:matricula.value,curso:curso.value,ativo:ativo.value});
  }else{
    const index = alunos.value.findIndex(aluno=>aluno.id===edit.value);

    if(index !=-1){
      alunos.value[index] = {id: edit.value, nome:nome.value,matricula:matricula.value,curso:curso.value,ativo:ativo.value};
    }
  }
  limpar();
 
}

function editar(aluno){
    edit.value = aluno.id;
    nome.value = aluno.nome;
    matricula.value = aluno.matricula;
    curso.value = aluno.curso;
    ativo.value = aluno.ativo;
    mostraForm.value = true;
  }
  function excluir (id){
    alunos.value = alunos.value.filter(aluno=>aluno.id!==id);
  }
  function limpar(){
    nome.value='';
    matricula.value='';
    curso.value='';
    ativo.value=true;
    edit.value=null;
  }
  

</script>