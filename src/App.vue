<template>
  <div>
    <h1>Cadastro de Alunos</h1>
    <p>Total de alunos: {{ alunos.length }}</p>

    <button @click="mostrarForm = !mostrarForm">
      {{ mostrarForm ? 'Ocultar Formulário' : 'Mostrar Formulário' }}
    </button>

    <div v-show="mostrarForm">
      <h2 v-if="editandoId === null">Cadastrar Aluno</h2>
      <h2 v-else>Editar Aluno</h2>

      Nome: <input v-model="nome" type="text" /><br />
      Matrícula: <input v-model="matricula" type="text" /><br />
      Curso: <input v-model="curso" type="text" /><br />
      Ativo: <input v-model="ativo" type="checkbox" /><br />

      <button @click="salvar">Salvar</button>
      <button @click="limpar">Cancelar</button>
    </div>

    <hr />

    <div v-if="alunos.length > 0">
      <ul>
        <li v-for="(a, i) in alunos" :key="a.id">
          #{{ i + 1 }} — {{ a.nome }} | {{ a.matricula }} | {{ a.curso }} |
          {{ a.ativo ? 'Ativo' : 'Inativo' }}

          <button @click="editar(a)">Editar</button>
          <button @click="excluir(a.id)">Excluir</button>
        </li>
      </ul>
    </div>
    <p v-else>Nenhum aluno cadastrado ainda.</p>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const alunos = ref([]);
const mostrarForm = ref(false);
const editandoId = ref(null);

const nome = ref('');
const matricula = ref('');
const curso = ref('');
const ativo = ref(true);

function salvar() {
  if (editandoId.value === null) {
    alunos.value.push({ id: Date.now(), nome: nome.value, matricula: matricula.value, curso: curso.value, ativo: ativo.value });
  } else {
    const index = alunos.value.findIndex(aluno => aluno.id === editandoId.value);
    if (index !== -1) {
      alunos.value[index] = { id: editandoId.value, nome: nome.value, matricula: matricula.value, curso: curso.value, ativo: ativo.value };
    }
  }
  limpar();
}

function editar(aluno) {
  editandoId.value = aluno.id;
  nome.value = aluno.nome;
  matricula.value = aluno.matricula;
  curso.value = aluno.curso;
  ativo.value = aluno.ativo;
  mostrarForm.value = true;
}

function excluir(id) {
  alunos.value = alunos.value.filter(aluno => aluno.id !== id);
}

function limpar() {
  nome.value = '';
  matricula.value = '';
  curso.value = '';
  ativo.value = true;
  editandoId.value = null;
}
</script>