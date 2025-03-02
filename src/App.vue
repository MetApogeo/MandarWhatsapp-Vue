<script setup>
import { ref, computed } from "vue";
import AgregarNumero from '@/components/MisComponentes/AgregarNumero.vue'

const listaContactos = ref([
  { id: 1, nombre: "Aaron", numero: "+529993312301" },
  { id: 2, nombre: "Jose Manuel", numero: "+524921763513" },
  { id: 3, nombre: "Carlos López", numero: "+521987654321" },
  { id: 4, nombre: "María Torres", numero: "+525512345678" },
]);

// Lista de códigos de país
const codigosPaises = [
  { nombre: "México", codigo: "+52" },
  { nombre: "Estados Unidos", codigo: "+1" },
  { nombre: "Argentina", codigo: "+54" },
  { nombre: "Colombia", codigo: "+57" },
  { nombre: "España", codigo: "+34" },
  { nombre: "Chile", codigo: "+56" },
  { nombre: "Perú", codigo: "+51" },
  { nombre: "Venezuela", codigo: "+58" },
];

// Variables reactivas
const codigoPais = ref("");
const numeroTelefono = ref(""); // Número de WhatsApp en formato internacional
const mensaje = ref("");






// Computed property para construir el enlace
const whatsappLink = computed(() => {
  if (!codigoPais.value || !numeroTelefono.value || !mensaje.value) return "#";
  const numeroCompleto = `${codigoPais.value}${numeroTelefono.value}`;
  const mensajeCodificado = encodeURIComponent(mensaje.value);
  return `https://wa.me/${numeroCompleto}?text=${mensajeCodificado}`;
});

const whatsappLink2 = (numero) => {
  const mensajeCodificado = encodeURIComponent(mensaje.value);
  return `https://wa.me/${numero}?text=${mensajeCodificado}`;
}

const agregarContacto = (nuevoContacto) => {
  listaContactos.value.push(nuevoContacto);
};

</script>

<template>
  <body>
    <AgregarNumero :contactos="listaContactos" @agregar-contacto="agregarContacto" />

    <div>
      <!-- Título -->
      <h3>Seleccionar contacto y enviar mensaje:</h3>

      <!-- Input de Mensaje -->
      <h3>Mensaje predeterminado:</h3>
      <input
        type="text"
        v-model="mensaje"
        placeholder="Escribe tu mensaje aquí"
      />

      <!-- Lista de contactos con botones "Enviar" -->
      <ul class="contact-list">
        <li v-for="contacto in listaContactos" :key="contacto.id">
          <div>
            <span>{{ contacto.nombre }} ({{ contacto.numero }})</span>
            <a
              :href="whatsappLink2(contacto.numero)"
              target="_blank"
              class="btn"
              :class="{ disabled: !mensaje }"
            >
              Enviar mensaje a WhatsApp
            </a>
          </div>
        </li>
      </ul>



      <br /><br />


    </div>
    <div>
      <h3>Seleccionar código de país:</h3>
      <select v-model="codigoPais" class="input-select">
        <option v-for="codigo in codigosPaises" :key="codigo.codigo" :value="codigo.codigo">
          {{ codigo.nombre }} ({{ codigo.codigo }})
        </option>
      </select>
      <h3>ingresar número: (máximo 10 dígitos)</h3>
      <input type="text" v-model="numeroTelefono" placeholder="Ej. 9991756865" maxlength="10">
      <h3>ingresar mensaje</h3>
      <input type="text" v-model="mensaje" placeholder="Escribe tu mensaje aqui">
    </div>
    <br><br>
    <div>
      <a :href="whatsappLink" target="_blank" class="btn" :class="{ disabled: !numeroTelefono || !mensaje || !codigoPais }">
        Enviar mensaje a WhatsApp
      </a>
    </div>
  </body>
</template>

<style scoped>
h3 {
  margin-bottom: 5px;
  font-size: 1rem;
}

.contact-list {
  list-style: none;
  padding: 0;
}

.contact-list li {
  margin-bottom: 10px;
}

label {
  font-size: 1rem;
}

input[type="checkbox"] {
  margin-right: 10px;
  transform: scale(1.2);
  cursor: pointer;
}

.input-select,
input[type="text"] {
  width: 100%;
  padding: 8px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-size: 1rem;
}

.btn {
  background-color: #25D366; /* Color de WhatsApp */
  color: white;
  padding: 10px 15px;
  border-radius: 5px;
  text-decoration: none;
  font-weight: bold;
  display: inline-block;
}
.btn:hover {
  background-color: #1ebe57;
}
.btn.disabled {
  background-color: gray;
  cursor: not-allowed;
}
</style>
