<script setup>
import { onMounted, ref } from 'vue';
import { RouterLink, useRoute } from 'vue-router';

const novoTutor = ref({
  nome:'',
  telefone:'',
  id:''
})
const router = useRoute

// aqui estou declarando a api que vai retornar a listagem do todos os pets
// sera responsavel por cadastrar novos pets
// POST para cadastrar novos pets salvar qualquer coisa
//GET (chamar todos)

/*   MÉTODOS HTTP  

 GET: recuperar id
 POST: salvar 
 PH/PATCH : atualizar id
 DELETE: remover id
*/

const API_URL = "http://localhost:3000";

//saber todos os tutores que existem
const tutores = ref({});

async function carregarTutores() {
  const resposta = await fetch (`${API_URL}/tutores`);
  console.log('load tutores', tutores);
  tutores.value = await resposta.json();
  
}

async function salvarTutor (){
  await fetch(`${API_URL}/tutores`,{
    method: 'POST', //salvar informação
    header:{
      'Content-type': 'application/json',
    },
    body: JSON.stringify(novoTutor.value),


    
  });
  router.push('/tutores')
}


onMounted(carregarTutores);
</script>

<template>
  <div>
    <header class="mb-4">
      <h1 class="text-2xl font-bold">Listagem de Tutores</h1>
      <p class="text-body-secondary mb-0">Cadastro de tutores no sistema.</p>
    </header>

    <RouterLink
      class="btn btn-primary"
      :to="{ name: 'addTutor' }"
    >
      Adicionar tutor
    </RouterLink>

    <p v-if="carregandoTutores">
       Carregando Tutores...
    </p>
    <div v-else>
      <p v-if="erro" class="alert alert-danger" role="alert">
      {{ erro }}
      </p>
    </div>

 <form @submit.prevent="salvarTutor">

  <div class="col-md-3"> <!-- coluna e tamanho da coluna -->
     <label for="nome" class="from-label">
       Nome do tutor
     </label>

     <input type="text"
      id="nome" 
      v-model="novoTutor.nome"
      class="form-control"
       required>
  </div>

   <div class="col-md-3"> 
     <label  for="telefone"  class="from-label">
      Número de telefone
     </label>

     <input type="tel"
      id="telefone" 
      v-model="novoTutor.telefone"
      class="form-control"
       required>


     </div>

      
 </form>

  </div>
</template>
