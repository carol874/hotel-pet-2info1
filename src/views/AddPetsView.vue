<script setup>
import { onMounted, ref } from 'vue';
import { RouterLink, useRoute } from 'vue-router';

const novoPet = ref({ //campos da tabela de pet para cadastrar
  nome:'',
  especie:'',
  tutorId: '',
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

async function salvarPet (){
  await fetch(`${API_URL}/pets`,{
    method: 'POST', //salvar informação
    header:{
      'Content-type': 'application/json',
    },
    body: JSON.stringify(novoPet.value),


    
  });
  router.push('/pets')
}

onMounted(carregarTutores);
</script>

<template>
  <div>
    <header class="mb-4">
      <h1 class="text-2xl font-bold">Listagem de Pets</h1>
      <p class="text-body-secondary mb-0">Cadastro de Pets no sistema.</p>
    </header>

    <RouterLink
      class="btn btn-primary"
      :to="{ name: 'addPet' }"
    >
      Adicionar Pet
    </RouterLink>

    <p v-if="carregandoTutores">
       Carregando Tutores...
    </p>
    <div v-else>
      <p v-if="erro" class="alert alert-danger" role="alert">
      {{ erro }}
      </p>
    </div>

 <form @submit.prevent="salvarPet">

  <div class="col-md-3"> <!-- coluna e tamanho da coluna -->
     <label for="nome" class="from-label">
       Nome do Pet
     </label>

     <input type="text"
      id="nome" 
      v-model="novoPet.nome"
      class="form-control"
       required>
  </div>

   <div class="col-md-3"> 
     <label for="nome" class="from-label">
      Espécie
     </label>

     <select  id="especie" v-model="novoPet.especie" class="form-select" required>
         <option value="" disabled>
            Selecione a espécie
         </option>
         
         <option value="Cachorro" >
          Cachorro
         </option>

         <option value="Gato" >
           Gato
         </option>
     </select>

     </div>

       <div class="col-md-3"> 
     <label for="nome" class="from-label">
      Tutor
     </label>

     <select  id="tutor" v-model="novoPet.tutorId" class="form-select" required>
         <option value="" disabled>
             Selecione um tutor
         </option>
         
         <option v-for="tutor in tutores"  :key="tutor.id" :value="tutor.id">
          {{ tutor.nome }}
         </option>
     </select>

     </div>



 </form>

  </div>
</template>
