<script setup lang="ts">
import { computed } from "vue";
import { IAlerta } from "../interfaces/alerta";
interface Props {
  alerta: IAlerta;
}

const { alerta } = defineProps<Props>();

const isError = computed( () => {
  return alerta.tipo  === 'ERROR'; 
})

</script>
<template>
  <div class="text-white text-center p-3 uppercase font-bold mb-3 rounded-md"
    :class="[isError ?  'bg-red-500' : 'bg-green-500' ]"
  >
    {{ alerta.mensaje }}
  </div>
</template>
