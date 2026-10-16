<script setup>
import { onMounted, ref } from 'vue';
import { RouterLink } from 'vue-router';

const API_URL = 'http://localhost:3000';

const pets = ref([]);
const tutores = ref([]);
const loading = ref(true);


async function carregarDados() {
  const respostaPets = await fetch(`${API_URL}/pets`);
  pets.value = await respostaPets.json();

  console.log('Pets:', pets.value);

  const respostaTutores = await fetch(`${API_URL}/tutores`);
  tutores.value = await respostaTutores.json();

  console.log('Tutores:', tutores.value);

  loading.value = false;
}

function nomeDoTutor(tutorId) { 
  for (const tutor of tutores.value) {
    console.log('tutor', tutorId);
    // tutor.id == tutorId
    if (tutor.id === tutorId) {
      return tutor.nome;
    }
  }
  return 'Tutor Não Encontrado!';
}
//por que ela depende do servidor, ela pode fazer varias ações juntas
async function excluirTutor(id) {
 const confirmarExclusão = window.confirm('Deseja realmente excluir o tutor?')

 if(confirmarExclusão){
   //lógica para excluir o pet
   await fetch(`${API_URL}/tutores/${id}`,{
    method: 'DELETE'
   });
   await carregarDados();
 }// recarrega os dados após a exclusão
  
}


// Recebe o tutorId do pet e procura o tutor correspondente.

onMounted(carregarDados);
</script>

<template>
  <div>
    <header class="mb-4">
      <h1 class="text-2xl font-bold">Listagem de Tutores</h1>
      <p class="text-body-secondary mb-0">
        Listagem dos Tutores cadastrados no sistema.
      </p>
    </header>

    <RouterLink
      class="btn btn-primary"
      :to="{ name: 'addTutores' }"
    >
      Adicionar Tutor
    </RouterLink>

    <table class="table table-striped table-hover">
      <thead>
        <tr>
          <th>ID</th>
          <th>Nome</th>
          <th>Telefone</th>
             <th>
            Ações
          </th>
        </tr>
      </thead>

      <tbody>
        <tr
          v-for="tutor in tutores"
          :key="tutor.id"
        >
          <td>{{ tutor.id }}</td>
          <td>{{ tutor.nome }}</td>
          <td>{{ tutor.telefone }}</td>
          
            <td>
            <RouterLink :to="`/tutores/${tutor.id}` ">
              Visualizar 
            </RouterLink>
            </td>


            <td>
             <button class="btn btn-danger" @click="excluirTutor(tutor.id) ">
             Excluir
             </button>
          </td>

        </tr>
      </tbody>
    </table>
  </div>
</template>
