<script setup lang="ts">
import { reactive } from "vue";
import { IAlerta } from "../interfaces/alerta";
import Alerta from "../components/Alerta.vue";

defineEmits([""]);

const alerta = reactive<IAlerta>({
  tipo: "",
  mensaje: "",
});

const validar = () => {
  if (Object.values(paciente).includes("")) {
    alerta.mensaje = "Todos los campos son obligatorios";
    alerta.tipo = "ERROR";
    return;
  }
};
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
        />
      </div>
      <input
        type="submit"
        class="bg-indigo-500 w-full p-3 text-white uppercase font-bold hover:bg-indigo-700 cursor-pointer transition-colors"
        value="Agregar paciente"
      />
    </form>
  </div>
</template>
