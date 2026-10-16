<script setup>
import { onMounted,ref  } from 'vue';
import { useRoute } from 'vue-router';
const pet = ref({});

const route = useRoute();
const API_URL = 'http://localhost:3000';
//exibir informaçãoes do pet e do tutor

const tutor = ref({});

async function carregarPet() {
    //exibindo dados do id do pet
    const idPet = route.params.id;
    console.log('ID do pet:', idPet);
    
    const respostaPet = await fetch (`${API_URL}/tutores/${idPet}`);
    pet.value = await respostaPet.json();

    const respostaTutor = await fetch (
        `${API_URL}/tutores/${pet.value.tutorId}`,
    );
    tutor.value = await respostaTutor.json();
}

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

onMounted (carregarPet);
</script>

<template>
      <h1>Detalhes do Tutor</h1>
      <p>
        Visualize dos dados do tutor e seus pets vinculados
      </p>

      <table class="table table-striped table-hover">
      <thead>
        <tr>
          <th>ID </th>
          
          <th>Nome Completo</th>
          <th>Telefone/ Celular/ WhatsApp</th>
             <th>
           Data de Nascimento
          </th>
          <th>
            E-mail
          </th>
          <th>
            CPF / CNPJ
          </th>
        </tr>
      </thead>


      </table>

     
      <button class="btn btn">
        <RouterLink :to="{name: 'tutores'}"  >
            Voltar para tutores
        </RouterLink>

      </button>

       <td>
             <button class="btn btn-danger" @click="excluirTutor(tutor.id) ">
             Remover Tutor
             </button>
          </td>
</template>