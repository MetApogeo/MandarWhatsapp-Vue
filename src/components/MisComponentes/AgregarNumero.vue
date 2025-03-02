<script setup>
import { ref, computed } from "vue";
import { defineEmits } from "vue";

const props = defineProps({
  contactos: {
    type: Array,
    required: true,
  }
});

const emit = defineEmits(['agregar-contacto']);
const nuevoNombre = ref("");
const nuevoNumero = ref("");

  const agregarContacto = () => {
    console.log(props.contactos);
  if(nuevoNombre.value && nuevoNumero.value){
    const nuevoId = props.contactos.length > 0
      ? Math.max(...props.contactos.map(contacto => contacto.id)) + 1
      : 1;
      const nuevoContacto =({
        id: nuevoId,
        nombre: nuevoNombre.value,
        numero: nuevoNumero.value,
      });
    emit("agregar-contacto", nuevoContacto);
    nuevoNombre.value = "";
    nuevoNumero.value = "";
    console.log(props.contactos.value);
  }
}

const isFormDisabled = computed (() => {
  return !nuevoNombre.value || !nuevoNumero.value;
})
</script>

<template>
  <div>
      <!-- Formulario para agregar nuevos contactos -->
      <h3>Agregar nuevo contacto:</h3>
      <div class="add-contact-form">
        <input
          type="text"
          v-model="nuevoNombre"
          placeholder="Nombre del contacto"
        />
        <input
          type="text"
          v-model="nuevoNumero"
          placeholder="Número de teléfono"
        />
        <button @click="agregarContacto" :disabled="isFormDisabled">Agregar</button>
      </div>
    </div>
</template>

<style scoped>

.input-select,
input[type="text"] {
  width: 100%;
  padding: 8px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-size: 1rem;
}

</style>
