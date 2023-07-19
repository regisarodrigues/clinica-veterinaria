<script setup>
import { computed, reactive } from 'vue';
import AlertForm from './AlertForm.vue';

const alerta = reactive({
  tipo: '',
  message: ''
});

const emit = defineEmits([
  'update:pet',
  'update:proprietario',
  'update:email',
  'update:alta',
  'update:sintomas',
  'salvar-paciente'
]);

const props = defineProps({
  id: {
    type: [String, null],
    required: true
  },
  pet: {
    type: String,
    required: true
  },
  proprietario: {
    type: String,
    required: true
  },
  email: {
    type: String,
    required: true
  },
  alta: {
    type: String,
    required: true
  },
  sintomas: {
    type: String,
    required: true
  }
});

const validar = () => {
  if (Object.values(props).includes('')) {
    alerta.message = 'Todos os campos são obrigatórios!';
    alerta.tipo = 'error';

    return;
  }

  emit('salvar-paciente');
  alerta.message = 'Paciente salvo com sucesso!';
  alerta.tipo = 'exito';

  setTimeout(() => {
    Object.assign(alerta, {
      tipo: '',
      message: ''
    });
  }, 3000);
};

const editando = computed(() => {
  return props.id;
});
</script>

<template>
  <div class="md:w-1/2">
    <h2 class="font-black text-3xl text-center">Pacientes</h2>
    <p class="text-lg mt-5 text-center mb-10">
      Cadastrar e <span class="text-indigo-600 font-bold">Administar</span>
    </p>

    <AlertForm v-if="alerta.message" :alerta="alerta" />

    <form
      class="bg-white shadow-md rounded-lg py-10 px-5 mb-10"
      @submit.prevent="validar"
      autocomplete="off"
    >
      <div class="mb-5">
        <label for="pet" class="block text-gray-700 uppercase font-bold"> Nome Pet </label>
        <input
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          type="text"
          id="pet"
          placeholder="Nome do pet"
          :value="pet"
          @input="$emit('update:pet', $event.target.value)"
        />
      </div>
      <div class="mb-5">
        <label for="proprietario" class="block text-gray-700 uppercase font-bold">
          Nome Proprietário
        </label>
        <input
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          type="text"
          id="proprietario"
          placeholder="Nome do proprietário"
          :value="proprietario"
          @input="$emit('update:proprietario', $event.target.value)"
        />
      </div>
      <div class="mb-5">
        <label for="email" class="block text-gray-700 uppercase font-bold"> Email </label>
        <input
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          type="email"
          id="email"
          placeholder="Email proprietário"
          :value="email"
          @input="$emit('update:email', $event.target.value)"
        />
      </div>
      <div class="mb-5">
        <label for="alta" class="block text-gray-700 uppercase font-bold">Data Alta </label>
        <input
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          type="date"
          id="alta"
          :value="alta"
          @input="$emit('update:alta', $event.target.value)"
        />
      </div>
      <div class="mb-5">
        <label for="sintomas" class="block text-gray-700 uppercase font-bold"> Sintomas </label>
        <textarea
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md h-20"
          id="sintomas"
          placeholder="Descrever os sintomas"
          :value="sintomas"
          @input="$emit('update:sintomas', $event.target.value)"
        />
      </div>
      <input
        class="bg-indigo-500 w-full p-3 text-white uppercase font-bold hover:bg-indigo-800 cursor-pointer transition-colors"
        type="submit"
        :value="[editando ? 'Salvar Alteração' : 'Registar Paciente']"
      />
    </form>
  </div>
</template>
