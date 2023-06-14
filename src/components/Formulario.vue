<script setup lang="ts">
import { TextareaHTMLAttributes, reactive, computed } from "vue";

import Alerta from "../components/Alerta.vue";

import { IAlerta } from "../interfaces/alerta";

interface Props {
  //debido al v-model los props se pasan automáticamente
  id: string | null;
  nombre: string;
  propietario: string;
  email: string;
  alta: string;
  sintomas: string;
}

const props = defineProps<Props>();

const alerta = reactive<IAlerta>({
  tipo: "",
  mensaje: "",
});

// update -> el v-model enlaza este nombre por defecto + nombre
// Hay que definir los emits que envia App.vue, ya que por defecto no están declarados
const emit = defineEmits([
  "update:nombre",
  "update:propietario",
  "update:email",
  "update:alta",
  "update:sintomas",
  "guardar-paciente", // custom event en <Formulario @guardar-paciente="guardarPaciente"></Formulario>
]);

const validar = () => {
  if (Object.values(props).includes("")) {
    alerta.mensaje = "Todos los campos son obligatorios";
    alerta.tipo = "ERROR";
    return;
  }

  emit("guardar-paciente"); //Emitir la funcion al padre
  alerta.mensaje = props.id
    ? "Paciente actualizado con éxito"
    : "Paciente guardado con éxito";
  alerta.tipo = "EXITO";

  setTimeout(() => {
    alerta.mensaje = "";
    alerta.tipo = "";
  }, 3000);
};

const editando = computed(() => props.id); // Mantener la reactividad
</script>

<template>
  <div class="md:w-1/2">
    <h1 class="font-black text-3xl text-center">Seguimiento de pacientes</h1>

    <p class="text-lg mt-5 text-center mb-10">
      Añade Pacientes y
      <span class="text-indigo-600 font-bold">Administralos</span>
    </p>

    <Alerta v-if="alerta.mensaje" :alerta="alerta"></Alerta>

    <form
      @submit.prevent="validar"
      class="bg-white shadow-md rounded-lg py-10 px-5 mb-10"
    >
      <div class="mb-5">
        <label for="mascota" class="block text-gray-700 uppercase font-bold"
          >Nombre mascota</label
        >
        <input
          type="text"
          id="mascota"
          class="w-full p-4 border-2 mt-2 placeholder-gray-400 rounded-md"
          placeholder="Nombre de la mascota"
          :value="nombre"
          @input="
            $emit('update:nombre', ($event.target as HTMLInputElement).value)
          "
        />
      </div>
      <div class="mb-5">
        <label for="propietario" class="block text-gray-700 uppercase font-bold"
          >Nombre propietario</label
        >
        <input
          type="text"
          id="propietario"
          class="w-full p-4 border-2 mt-2 placeholder-gray-400 rounded-md"
          placeholder="Nombre del propietario"
          :value="propietario"
          @input="
            $emit(
              'update:propietario',
              ($event.target as HTMLInputElement).value
            )
          "
        />
      </div>
      <div class="mb-5">
        <label for="email" class="block text-gray-700 uppercase font-bold"
          >Email</label
        >
        <input
          type="email"
          id="email"
          class="w-full p-4 border-2 mt-2 placeholder-gray-400 rounded-md"
          placeholder="Email del propietario"
          :value="email"
          @input="
            $emit('update:email', ($event.target as HTMLInputElement).value)
          "
        />
      </div>
      <div class="mb-5">
        <label for="alta" class="block text-gray-700 uppercase font-bold"
          >Alta</label
        >
        <input
          type="date"
          id="alta"
          class="w-full p-4 border-2 mt-2 placeholder-gray-400 rounded-md"
          :value="alta"
          @input="
            $emit('update:alta', ($event.target as HTMLInputElement).value)
          "
        />
      </div>
      <div class="mb-5">
        <label for="sintomas" class="block text-gray-700 uppercase font-bold"
          >Síntomas</label
        >
        <textarea
          id="sintomas"
          class="w-full p-4 border-2 mt-2 placeholder-gray-400 rounded-md h-40"
          placeholder="Describe los síntomas del paciente"
          :value="sintomas"
          @input="
            $emit(
              'update:sintomas',
              ($event.target as TextareaHTMLAttributes).value
            )
          "
        />
      </div>
      <input
        type="submit"
        class="bg-indigo-500 w-full p-3 text-white uppercase font-bold hover:bg-indigo-700 cursor-pointer transition-colors"
        :value="[editando ? 'Actualizar paciente' : 'Agregar paciente']"
      />
    </form>
  </div>
</template>
