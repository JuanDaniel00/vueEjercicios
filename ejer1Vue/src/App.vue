<template>
  <div class="mainDiv">
    <div class="divForm">
      <div class="divForm1 formsDiv">
        <div class="divName divInput">
          <label for="name">Nombre</label>
          <input type="text" id="name" v-model="nombre" />
        </div>
        <div class="divLastName divInput">
          <label for="lastName">Apellido</label>
          <input type="text" id="lastName" v-model="apellido" />
        </div>
      </div>
      <div class="divForm2 formsDiv">
        <div class="divBirthdate divInput">
          <label for="birthdate">Fecha de Nacimiento</label>
          <input type="date" id="birthdate" v-model="fechaNac" />
        </div>
      </div>

      <div class="divForm3 formsDiv">
        <div class="divEmail divInput">
          <label for="email">Correo Electronico</label>
          <input type="email" id="email" v-model="correo" />
        </div>
      </div>

      <div class="divForm4 formsDiv">
        <div class="divButton divInput">
          <button @click="validacion()">Enviar</button>
        </div>
      </div>
    </div>
    <div class="divTable">
      <table>
        <thead>
          <tr>
            <th>Nombre</th>
            <th>Apellido</th>
            <th>Fecha Nacimiento</th>
            <th>Correo Electronico</th>
            <th>Edad</th>
            <th>Opciones</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(usuario, i) in usuarios" :key="i">
            <td>{{ usuario.nombre }}</td>
            <td>{{ usuario.apellido }}</td>
            <td>{{ usuario.fechaNac }}</td>
            <td>{{ usuario.correo }}</td>
            <td>{{ usuario.edadAct }}</td>
            <td @click="borrarFila(usuario)"><button>❌</button></td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

let usuarios = ref([]);

let nombre = ref("");
let apellido = ref("");
let fechaNac = ref("");
let correo = ref("");
let edadAct = ref("");

function validacion() {
  if (
    nombre.value === "" ||
    apellido.value === "" ||
    fechaNac.value === "" ||
    correo.value === ""
  ) {
    alert("Por favor, llene todos los campos");
    return;
  }

  if (correo.value.indexOf("@") === -1 || correo.value.indexOf(".") === -1) {
    alert("Correo invalido");
    return;
  }

  if (new Date(fechaNac.value) > new Date()) {
    alert("Fecha de nacimiento invalida");
    return;
  }

  enviar();
}

const enviar = () => {
  let edad = new Date().getFullYear() - new Date(fechaNac.value).getFullYear();
  edadAct.value = edad;
  usuarios.value.push({
    nombre: nombre.value,
    apellido: apellido.value,
    fechaNac: fechaNac.value,
    correo: correo.value,
    edadAct: edadAct.value,
  });
  limpiarCampos();
};

const borrarFila = (usuario) => {
  usuarios.value.splice(usuarios.value.indexOf(usuario), 1);
};

function limpiarCampos() {
  nombre.value = "";
  apellido.value = "";
  fechaNac.value = "";
  correo.value = "";
}
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
}

.mainDiv {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.divForm {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 50%;
}

.formsDiv {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
}

.divInput {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 100%;
}

.divName,
.divLastName,
.divBirthdate,
.divEmail,
.divButton {
  margin: 10px;
}

.divTable {
  width: 50%;
}

table {
  width: 100%;
  border-collapse: collapse;
}

th,
td {
  border: 1px solid black;
  padding: 10px;
  text-align: center;
}
</style>