<script setup>
import { reactive, ref, onMounted, watch } from 'vue';

import { uid } from 'uid';

import Header from './components/Header.vue';
import Form from './components/Form.vue';
import Paciente from './components/Paciente.vue';

const pacientes = ref([]);

const paciente = reactive({
  id: null,
  nome: '',
  dono: '',
  email: '',
  data: '',
  sintomas: ''
});

const salvarPaciente = () => {
  if(paciente.id) {
    const { id } = paciente;
    const i = pacientes.value.findIndex(pacienteState => pacienteState.id === id);
    pacientes.value[i] = {...paciente}
  } else {
    pacientes.value.push({
      ...paciente,
      id: uid(),
    });
  }

  // reiniciando o objeto
  Object.assign(paciente, {
    nome: '',
    dono: '',
    email: '',
    data: '',
    sintomas: '',
    id: null
  });
}

const atualizarPaciente = (id) => {
  const editar = pacientes.value.filter(paciente => paciente.id === id)[0];
  Object.assign(paciente, editar);
}

const eliminarPaciente = (id) => {
  pacientes.value = pacientes.value.filter(paciente => paciente.id !== id);
}

onMounted(() => {
  // Carregue os pacientes do LocalStorage ao iniciar o componente
  const pacientesLocalStorage = localStorage.getItem('pacientes');
  if (pacientesLocalStorage) {
    pacientes.value = JSON.parse(pacientesLocalStorage);
  }
});

watch(pacientes, () => {
  // Salve os pacientes no LocalStorage sempre que houver alterações
  localStorage.setItem('pacientes', JSON.stringify(pacientes.value));
}, {
  deep: true,
});


</script>

<template>
  <div class="container mx-auto mt-20">
    <Header />

    <div class="mt-12 mx-10 md:flex">
      <Form
      v-model:nome="paciente.nome"
      v-model:dono="paciente.dono"
      v-model:email="paciente.email"
      v-model:data="paciente.data"
      v-model:sintomas="paciente.sintomas"
      @salvar-paciente="salvarPaciente"
      :id="paciente.id"
      />
      
      <div class="md:w-1/2 md:h-screen overflow-y-scroll">
        <h3 class="font-black text-3xl text-center">Lista de Pacientes</h3>
        <p class="text-lg mt-5 text-center mb-10">
            Pacientes
            <span class="text-indigo-600 font-bold">cadastrados.</span>
        </p>
        <div v-if="pacientes.length > 0">
          <Paciente
            v-for="(paciente, index) in pacientes" :key="index"
            :paciente="paciente"
            @atualizar-paciente="atualizarPaciente"
            @eliminar-paciente="eliminarPaciente"
          />
        </div>
        <p v-else class="mt-10 text-center text-2xl">Sem pacientes registrados.</p>
      </div>
    </div>
  </div>

</template>