<template>
  <div class="mainDiv">
    <div class="divFixed">
      <div class="divPalabras">
        <div>
          <h1>
            Precio total del inventario:
            {{
              productos.reduce(
                (acc, producto) => acc + producto.precio * producto.cantidad,
                0
              )
            }}
          </h1>
        </div>
        <div>
          <h1>
            Costo total del inventario:
            {{
              productos.reduce(
                (acc, producto) => acc + producto.costo * producto.cantidad,
                0
              )
            }}
          </h1>
        </div>
        <div>
          <h1>
            Ganancia total:
            {{
              productos.reduce(
                (acc, producto) => acc + producto.ganancias * producto.cantidad,
                0
              )
            }}
          </h1>
        </div>
      </div>
    </div>
    <!-- Button trigger modal -->
    <button
      type="button"
      class="btn btn-primary"
      data-bs-toggle="modal"
      data-bs-target="#exampleModal"
    >
      Abrir Formulario <i class="bi bi-x-lg"></i>
    </button>

    <!-- Modal -->
    <div
      class="modal fade"
      id="exampleModal"
      tabindex="-1"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h1 class="modal-title fs-5" id="exampleModalLabel">Formulario</h1>
            <button
              type="button"
              class="btn-close button3"
              data-bs-dismiss="modal"
              aria-label="Close"
            >
              <span class="X"></span>
              <span class="Y"></span>
            </button>
          </div>
          <div class="modal-body">
            <div class="divForm">
              <div class="divForm1 formsDiv">
                <div class="divName divInput">
                  <label for="name">Digite el nombre del producto</label>
                  <input type="text" id="name" v-model="nombre" />
                </div>
                <div class="divPrice divInput">
                  <label for="lastName">Digite el precio</label>
                  <input type="text" id="precio" v-model.number="precio" />
                </div>
                <div class="divCost divInput">
                  <label for="birthdate">Digite el costo</label>
                  <input type="text" id="costo" v-model.number="costo" />
                </div>
                <div class="divSupplier divInput">
                  <label for="birthdate">Digite el nombre del proveedor</label>
                  <input type="text" id="proveedor" v-model="proveedor" />
                </div>
              </div>
            </div>
          </div>
          <div class="modal-footer">
            <button
              type="button"
              class="btn btn-secondary"
              data-bs-dismiss="modal"
            >
              Cerrar
            </button>
            <div class="divButton divInput">
              <button class="btn" @click="validacion()">Agregar</button>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="divTable">
      <table>
        <thead>
          <tr>
            <th>Nombre</th>
            <th>Precio</th>
            <th>Costo</th>
            <th>Cantidad</th>
            <th>proveedor</th>
            <th></th>
            <th>Ganancias</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(producto, i) in productos" :key="i">
            <td>{{ producto.nombre }}</td>
            <td>{{ producto.precio }}</td>
            <td>{{ producto.costo }}</td>
            <td>{{ producto.cantidad }}</td>
            <td>{{ producto.proveedor }}</td>
            <td>
              <button @click="aumentarCant(producto)">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  x="0px"
                  y="0px"
                  width="100"
                  height="100"
                  viewBox="0 0 48 48"
                >
                  <path
                    fill="#4caf50"
                    d="M44,24c0,11.045-8.955,20-20,20S4,35.045,4,24S12.955,4,24,4S44,12.955,44,24z"
                  ></path>
                  <path fill="#fff" d="M21,14h6v20h-6V14z"></path>
                  <path fill="#fff" d="M14,21h20v6H14V21z"></path>
                </svg>
              </button>
              <button @click="disminuirCant(producto)">
                <img
                  src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADAAAAAwCAYAAABXAvmHAAAACXBIWXMAAAsTAAALEwEAmpwYAAACb0lEQVR4nO1ZzW7TQBBeAeXAEcqJwluAyokXqFQ8mxV/99xbdSaBi29t7xQVxCMg4ADlwgv0StU+ANAT/RG0O1bSi6tJqiAajO2s43WQP2mkSLbj71uPZ78ZK1WjRg1nxMZcjMzSXavxmQV8y0A7DHhogU4k5DcDbssxOYcDmo3D8ILyjWh+8SYDrVqgXdYU5wmr8TtrXOGAZkonfmSeXmdNryxgNy/xISGAXQu0fvRocboU8sfQesxAB67EhwP3owY9HBvxuNmcYqDXxROn80/kpdyrWPJz4RUG/DRu8vw7NuSeBa58qeTjs/gcm/Cys4Ay0oYT04nWncgfN/CJL/J8FhHgg5HI/7rfvsZAe74FMNDBSCVW6rx38nqQSi/ykQ9opohNqjgB2I1M61Z2AUCrvknzUOBKJvJisvo+xTdhOp9Gu2IaUwWIq/RNlpPC0J1UAT2765uoTnoKrXYGAfQu6Q9ONndKCU4SoPFNqgBpPKoqgAG3MghItsr+BdBeegr9o/77FmA1df5/ATzpKcST/hLbSS+jtsobmcZWegoFNOubKCdF0L6d0czRt8qtPuDXzNO83sSsAqT5j8DlTOQr2dBo6rBZuKHyQCYC1RGAz1Ve/DQLVyvR1GvcH3luKrNK3wKiBhrlAplVeksdwDXlCulDrab3HlLnY3wvvOQsYDDc1bRRGnmgD4UNdwcims2pMiqTBVwrbOX/BplVjqU6Af5wfmHzzE1l3NfbYFxXXFNH6ryUbVU2ZHcU2zGKd+pfg8u5d9hxQEyWDJ1kbiOeXRoP6ez6H/Cw2+vygL7IMbHE4ior8Zm1Rg01+TgFI5yW29LO/hIAAAAASUVORK5CYII="
                />
              </button>
            </td>
            <td>
              {{ producto.ganancias * producto.cantidad }}
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

let productos = ref([]);

let nombre = ref("");
let precio = ref("");
let costo = ref("");
let cantidad = ref("1");
let proveedor = ref("");
let ganancias = ref("");

function validacion() {
  if (
    nombre.value === "" ||
    precio.value < 0 ||
    costo.value < 0 ||
    proveedor.value === ""
  ) {
    Swal.fire({
      title: "No pueden quedar campos vacios",
      showClass: {
        popup: `
      animate__animated
      animate__fadeInUp
      animate__faster
    `,
      },
      hideClass: {
        popup: `
      animate__animated
      animate__fadeOutDown
      animate__faster
    `,
      },
    });
    return;
  }
  if (precio.value < costo.value) {
    Swal.fire({
      title: "El precio no puede ser menor al costo",
      showClass: {
        popup: `
      animate__animated
      animate__fadeInUp
      animate__faster
    `,
      },
      hideClass: {
        popup: `
      animate__animated
      animate__fadeOutDown
      animate__faster
    `,
      },
    });
    return;
  }
  agregar();
}

const agregar = () => {
  productos.value.push({
    nombre: nombre.value,
    precio: precio.value,
    costo: costo.value,
    cantidad: cantidad.value,
    proveedor: proveedor.value,
    ganancias: precio.value - costo.value,
  });
  limpiarCampos();
};

const aumentarCant = (producto) => {
  producto.cantidad++;
};

const disminuirCant = (producto) => {
  if (producto.cantidad > 0) {
    producto.cantidad--;
  }
};

const limpiarCampos = () => {
  nombre.value = "";
  precio.value = "";
  costo.value = "";
  proveedor.value = "";
  ganancias.value = "";
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
}

svg {
  width: 20px;
  height: 20px;
}
img {
  width: 20px;
  height: 20px;
}

.modal-header {
  border-bottom: 2px solid #4caf50;
}

.modal-footer {
  border-top: 2px solid #4caf50;
}

body {
  background-color: #242424 !important;
}

.divFixed {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  background-color: #f1f1f110;
  color: #45a049;
}

.modal-content {
  background: #242424 !important;
}
.modal-title {
  color: antiquewhite;
}
.btn-close {
  color: #c91a1a !important;
}

label {
  color: antiquewhite;
}

.btn-primary {
  position: fixed;
  bottom: 10px;
  right: 10px;
}

.divPalabras {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: row;
  gap: 15%;
  margin: 10px;
}

.button3 {
  position: relative;
  width: 2em;
  height: 2em;
  border: none;
  background: #242424;
  border-radius: 5px;
  transition: background 0.5s;
}

.X {
  content: "";
  position: absolute;
  top: 50%;
  left: 50%;
  width: 1.5em;
  height: 1.5px;
  background-color: #fff;
  transform: translateX(-50%) rotate(45deg);
}

.Y {
  content: "";
  position: absolute;
  top: 50%;
  left: 50%;
  width: 1.5em;
  height: 1.5px;
  background-color: #fff;
  transform: translateX(-50%) rotate(-45deg);
}

.close {
  position: absolute;
  display: flex;
  padding: 0.8rem 1.5rem;
  align-items: center;
  justify-content: center;
  transform: translateX(-50%);
  top: -70%;
  left: 50%;
  width: 3em;
  height: 1.7em;
  font-size: 12px;
  background-color: #242424;
  color: rgb(187, 229, 236);
  border: none;
  border-radius: 3px;
  pointer-events: none;
  opacity: 0;
}

.button3:hover {
  background-color: rgb(211, 21, 21);
}

.button3:active {
  background-color: rgb(130, 0, 0);
}

.button3:hover > .close {
  animation: close 0.2s forwards 0.25s;
}

@keyframes close {
  100% {
    opacity: 1;
  }
}

.divPalabras h1 {
  color: #45a049;
  font-size: 1.2em;
}

.mainDiv {
  display: flex;
  align-items: center;
  height: 100vh;
  width: 100vw;
}

.divForm {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
}

.divForm1 {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 50%;
}

.divForm2 {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 50%;
}

.formsDiv {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  width: 100%;
}

.divInput {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  margin: 10px;
}

.divTable {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  position: absolute;
  top: 53px;
}

#app {
  margin: 0;
  padding: 0;
}

table {
  width: 100%;
  border-collapse: collapse;
}

th {
  background-color: #a7bb8c;
  color: black;
}

th,
td {
  border: 1px solid black;
  padding: 10px;
}

td {
  color: bisque;
}

button {
  background-color: #242424;
}

.btn {
  background-color: #4caf50;
  color: white;
  padding: 10px 20px;
  border: none;
  cursor: pointer;
}
.btn:hover {
  background-color: #45a049;
}
</style>