<script setup>
import { onMounted, ref } from 'vue';
import { RouterLink, useRouter } from 'vue-router';

const novoTutor = ref({
  nome:'',
  telefone:'',
  id:'',
   cpf:'',
    data:'',
     email:''
})
const router = useRouter();

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
    headers:{
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
      <h1 class="text-2xl font-bold">Cadastar Tutores</h1>
      <p class="text-body-secondary mb-0">Preencha os dados para cadastrar um novo tutor</p>
    </header>


    <p v-if="carregandoTutores">
       Carregando Tutores...
    </p>
    <div v-else>
      <p v-if="erro" class="alert alert-danger" role="alert">
      {{ erro }}
      </p>
    </div>

 <form type="submit" @submit.prevent="salvarTutor">
    
  <div>
  <div class="col-md-3"> 
     <label  for="id"  class="from-label" >
      ID
     </label>

     <input type="tel"
      id="id"
      v-model="novoTutor.id"
      class="form-control"
      placeholder="Gerado automaticamente"
     disabled>
     </div>

  <div class="col-md-3"> <!-- coluna e tamanho da coluna -->
     <label for="nome" class="from-label" >
       Nome Completo
     </label>

     <input type="text"
      id="nome" 
      v-model="novoTutor.nome"
      class="form-control"
       placeholder="Digite o nome completo"
       required>
  </div>

   <div class="col-md-3"> 
     <label  for="telefone"  class="from-label" >
      Telefone / Celular/ WhatsApp
     </label>

     <input type="tel"
      id="telefone" 
      v-model="novoTutor.telefone"
      class="form-control"
       placeholder="(00) 00000-0000"
       required>
     </div>

     </div>

     <div>
     <div class="col-md-3"> 
     <label  for="cpf"  class="from-label" >
     CPF / CNPJ
     </label>

     <input type=""
      id="cpf" 
      v-model="novoTutor.cpf"
      class="form-control"
       placeholder="Digite o CPF ou CNPJ"
       required>
     </div>

      <div class="col-md-3"> 
     <label  for="data"  class="from-label" >
     Data de Nascimento
     </label>

     <input type="month"
      id="data" 
      v-model="novoTutor.data"
      class="form-control"
       placeholder="dd/mm/aa"
       required>
     </div>

           <div class="col-md-3"> 
     <label  for="data"  class="from-label" >
     Data de Nascimento
     </label>

     <input type="email"
      id="email" 
      v-model="novoTutor.email"
      class="form-control"
       placeholder="nome@example.com"
       required>
     </div>
     </div>

      
 </form>
     <button  class="btn btn-primary" @click="salvarTutor">Cadastrar Tutor</button>
  </div>
</template>
