<script setup>
import FormularioCadastro from '@/components/FormularioCadastro.vue';
import HeaderComponent from '@/components/HeaderComponent.vue';
import ListPaciente from '@/components/ListPaciente.vue';
import { uid } from 'uid';
import { onMounted, reactive, ref, watch } from 'vue';

const pacientes = ref([]);

const paciente = reactive({
  id: null,
  pet: '',
  proprietario: '',
  email: '',
  alta: '',
  sintomas: ''
});

const salvarPaciente = () => {
  if (paciente.id) {
    const { id } = paciente;
    const i = pacientes.value.findIndex((pacienteStage) => pacienteStage.id === id);
    pacientes.value[i] = { ...paciente };
  } else {
    pacientes.value.push({ ...paciente, id: uid() });
  }

  Object.assign(paciente, {
    pet: '',
    proprietario: '',
    email: '',
    alta: '',
    sintomas: '',
    id: null
  });
};

watch(
  pacientes,
  () => {
    salvarLocalStorage();
  },
  {
    deep: true
  }
);

const salvarLocalStorage = () => {
  localStorage.setItem('pacientes', JSON.stringify(pacientes.value));
};

onMounted(() => {
  const pacientesStorage = localStorage.getItem('pacientes');
  if (pacientesStorage) pacientes.value = JSON.parse(pacientesStorage);
});

const atualizarPaciente = (id) => {
  const editarPaciente = pacientes.value.filter((paciente) => paciente.id === id)[0];
  Object.assign(paciente, editarPaciente);
};

const deletarPaciente = (id) => {
  pacientes.value = pacientes.value.filter((paciente) => paciente.id !== id);
};
</script>

<template>
  <div class="container mx-auto mt-20">
    <HeaderComponent />
  </div>
  <div class="mt-12 md:flex">
    <FormularioCadastro
      v-model:pet="paciente.pet"
      v-model:proprietario="paciente.proprietario"
      v-model:email="paciente.email"
      v-model:alta="paciente.alta"
      v-model:sintomas="paciente.sintomas"
      @salvar-paciente="salvarPaciente"
      :id="paciente.id"
    />

    <div class="md:w-1/2 md:h-screen overflow-y-scroll">
      <h3 class="font-black text-3xl text-center">Administrar Pacientes</h3>
      <div v-if="pacientes.length > 0">
        <p class="text-lg mt-5 text-center mb-10">
          Informação do <span class="text-indigo-600 font-bold">Paciente</span>
        </p>
        <ListPaciente
          v-for="paciente in pacientes"
          :key="paciente"
          :paciente="paciente"
          @atualizar-paciente="atualizarPaciente"
          @deletar-paciente="deletarPaciente"
        />
      </div>
      <p class="mt-20 text-2xl text-center" v-else>Não tem Pacientes!</p>
    </div>
  </div>
</template>
