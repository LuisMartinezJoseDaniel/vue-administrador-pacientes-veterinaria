<script setup lang="ts">
import { reactive, ref } from "vue";

import Header from "./components/Header.vue";
import Formulario from "./components/Formulario.vue";
import { IPaciente } from "./interfaces/paciente";

const pacientes = ref<IPaciente[]>([]);

const paciente = reactive<IPaciente>({
  nombre: "",
  propietario: "",
  email: "",
  alta: "",
  sintomas: "",
});
</script>

<template>
  <div class="container mx-auto mt-20">
    <Header></Header>

    <div class="mt-12 md:flex">
      <Formulario
        v-model:nombre="paciente.nombre"
        v-model:propietario="paciente.propietario"
        v-model:email="paciente.email"
        v-model:alta="paciente.alta"
        v-model:sintomas="paciente.sintomas"
      ></Formulario>

      <div class="md:w-1/2 md:h-screen overflow-y-scroll">
        <h3 class="font-black text-3xl text-center">
          Administra tus pacientes
        </h3>

        <div class="" v-if="pacientes.length > 0"></div>
        <p v-else class="mt-20 text-2xl text-center">No hay pacientes</p>
      </div>
    </div>
  </div>
</template>
