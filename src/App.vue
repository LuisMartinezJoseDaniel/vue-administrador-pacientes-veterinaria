<script setup lang="ts">
import { onMounted, reactive, ref, watch } from "vue";

import { uid } from "uid";

import Header from "./components/Header.vue";
import Formulario from "./components/Formulario.vue";
import Paciente from "./components/Paciente.vue";

import { IPaciente } from "./interfaces/paciente";

const initialPaciente: IPaciente = {
  id: null,
  nombre: "",
  propietario: "",
  email: "",
  alta: "",
  sintomas: "",
};

const pacientes = ref<IPaciente[]>([]);

const paciente = reactive<IPaciente>({
  id: null,
  nombre: "",
  propietario: "",
  email: "",
  alta: "",
  sintomas: "",
});

const guardarPaciente = () => {
  if (paciente.id) {
    const pacienteIndex = pacientes.value.findIndex(
      (p) => p.id === paciente.id
    );
    if (pacienteIndex === -1) return;

    pacientes.value.splice(pacienteIndex, 1, { ...paciente });
  } else {
    pacientes.value.push({ ...paciente, id: uid() });
  }

  //Reasignar los valores para limpiar el form
  Object.assign(paciente, initialPaciente);
};

const actualizarPaciente = (id: string) => {
  const existePaciente = pacientes.value.find((p) => p.id === id);
  if (!existePaciente) return;

  Object.assign(paciente, existePaciente);
};

const eliminarPaciente = (id: string) => {
  const isConfirmed = confirm(
    `Estas seguro de eliminar al paciente con el id = [${id}]`
  );
  if (!isConfirmed) return;
  pacientes.value = pacientes.value.filter((p) => p.id !== id);
};

const guardarLocalStorage = () => {
  localStorage.setItem("pacientes", JSON.stringify(pacientes.value));
};

//Similar a useEffect,
watch(pacientes, () => guardarLocalStorage(), {
  deep: true, // Escuechar por los cambios de los atributos
});
onMounted(() => {
  if (!localStorage.getItem("pacientes")) return;
  pacientes.value = JSON.parse(localStorage.getItem("pacientes")!);
});
</script>

<template>
  <div class="container mx-auto mt-20">
    <Header></Header>

    <div class="mt-12 md:flex">
      <Formulario
        v-model:id="paciente.id"
        v-model:nombre="paciente.nombre"
        v-model:propietario="paciente.propietario"
        v-model:email="paciente.email"
        v-model:alta="paciente.alta"
        v-model:sintomas="paciente.sintomas"
        @guardar-paciente="guardarPaciente"
      ></Formulario>

      <div class="md:w-1/2 md:h-screen overflow-y-scroll">
        <h3 class="font-black text-3xl text-center">
          Administra tus pacientes
        </h3>

        <div class="" v-if="pacientes.length > 0">
          <p class="text-lg mt-5 text-center mb-10">
            Información de
            <span class="text-indigo-600 font-bold">Pacientes</span>
          </p>
          <Paciente
            v-for="paciente in pacientes"
            :paciente="paciente"
            @actualizar-paciente="actualizarPaciente"
            @eliminar-paciente="eliminarPaciente"
          ></Paciente>
        </div>
        <p v-else class="mt-20 text-2xl text-center">No hay pacientes</p>
      </div>
    </div>
  </div>
</template>
