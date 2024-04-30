<template>
  <div class="mainDiv">
    <div class="divForm">
      <div class="divForm1 formsDiv">
        <div class="divName divInput">
          <label for="name">Nombre</label>
          <input
            type="text"
            id="name"
            v-model="nombre"
            placeholder="Nombre"
            class="input"
            name="text"
          />
        </div>
        <div class="divLastName divInput">
          <label for="lastName">Apellido</label>
          <input
            type="text"
            id="lastName"
            v-model="apellido"
            placeholder="Apellido"
            class="input"
            name="text"
          />
        </div>
      </div>
      <div class="divForm2 formsDiv">
        <div class="divBirthdate divInput">
          <label for="birthdate">Fecha de Nacimiento</label>
          <input
            type="date"
            id="birthdate"
            v-model="fechaNac"
            class="input"
            name="text"
          />
        </div>
      </div>

      <div class="divForm3 formsDiv">
        <div class="divEmail divInput">
          <label for="email">Correo Electronico</label>
          <input
            type="email"
            id="email"
            v-model="correo"
            placeholder="Correo Electronico"
            class="input"
            name="text"
          />
        </div>
      </div>

      <div class="divForm4 formsDiv">
        <div class="divButton divInput">
          <button @click="validacion()">
            <div class="svg-wrapper-1">
              <div class="svg-wrapper">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 24 24"
                  width="24"
                  height="24"
                >
                  <path fill="none" d="M0 0h24v24H0z"></path>
                  <path
                    fill="currentColor"
                    d="M1.946 9.315c-.522-.174-.527-.455.01-.634l19.087-6.362c.529-.176.832.12.684.638l-5.454 19.086c-.15.529-.455.547-.679.045L12 14l6-8-8 6-8.054-2.685z"
                  ></path>
                </svg>
              </div>
            </div>
            <span>Send</span>
          </button>
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

.input {
  background-color: #212121;
  padding: 10px;
  /* text-align: center; */
  border: 2px solid white;
  border-radius: 5px;
  /* box-shadow: 3px 3px 2px rgb(249, 255, 85); */
}

.input:focus {
  color: rgb(0, 255, 255);
  background-color: #212121;
  outline-color: rgb(0, 255, 255);
  box-shadow: -3px -3px 15px rgb(0, 255, 255);
  transition: .1s;
  transition-property: box-shadow;
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
button {
  font-family: inherit;
  font-size: 20px;
  background: royalblue;
  color: white;
  padding: 0.7em 1em;
  padding-left: 0.9em;
  display: flex;
  align-items: center;
  border: none;
  border-radius: 16px;
  overflow: hidden;
  transition: all 0.2s;
  cursor: pointer;
}

button span {
  display: block;
  margin-left: 0.3em;
  transition: all 0.3s ease-in-out;
}

button svg {
  display: block;
  transform-origin: center center;
  transition: transform 0.3s ease-in-out;
}

button:hover .svg-wrapper {
  animation: fly-1 0.6s ease-in-out infinite alternate;
}

button:hover svg {
  transform: translateX(1.2em) rotate(45deg) scale(1.1);
}

button:hover span {
  transform: translateX(5em);
}

button:active {
  transform: scale(0.95);
}

@keyframes fly-1 {
  from {
    transform: translateY(0.1em);
  }

  to {
    transform: translateY(-0.1em);
  }
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