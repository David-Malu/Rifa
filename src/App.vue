<template>
  <div>
    <div class="container-fluid" id="fondo">
      <div class="row">
        <div class="col-sm-2"></div>

        <div class="col-sm-8 d-flex justify-content-center align-items-center">
          <div style="text-align: center">
            <h1 id="titulo" style="color: rgb(255, 255, 255)">SuerteOnline.com</h1>
            <p style="margin-top: 20px;" >
              "Descubre una manera emocionante de ganar premios. Nuestra
              plataforma es segura y fácil de usar.
              <b
                >¡No pierdas la oportunidad de ser uno de nuestros afortunados
                ganadores!</b
              >"
            </p>
          </div>
        </div>

        <div class="col-sm-2"></div>
      </div>

      <div class="row">
        <div class="col-sm-2"></div>
        <div class="col-sm-9">
          <div class="d-grid gap-2">
            <button
              type="button"
              id="sombra"
              class="btn btn-dark mt-5"
              data-bs-toggle="modal"
              data-bs-target="#exampleModal"
            >
              Informacion de las boletas
            </button>
          </div>
        </div>
        <div class="col-sm-1"></div>
      </div>

      <div class="row mt-2">
        <div class="col-sm-1"></div>
        <div class="col-sm-1">
          <h3><b>Boletas:</b></h3>
        </div>
        <div class="col-sm-9" id="botones">
          <button
            v-bind:style="
              c.estado == '1'
                ? 'background-color:red'
                : c.estado == '2'
                ? 'background-color:green'
                : 'background-color:blue'
            "
            id="centrar"
            data-bs-toggle="offcanvas"
            data-bs-target="#offcanvasBottom"
            type="button"
            aria-controls="offcanvasBottom"
            v-for="(c, i) in clientes"
            :key="i"
            @click="editar(c, i)"
          >
            {{ i }}
          </button>
        </div>
        <div class="col-sm-1"></div>
      </div>

      <div
        class="offcanvas offcanvas-bottom"
        tabindex="-1"
        id="offcanvasBottom"
        aria-labelledby="offcanvasBottomLabel"
      >
        <div class="offcanvas-header">
          <h5 class="offcanvas-title" id="offcanvasBottomLabel">
            <p>
              Informacion Del Boleto: <b>{{ id }}</b>
            </p>
          </h5>
          <div
            id="letra"
            role="alert"
            v-if="fl == false"
            style="width: 50%; height: 15px; color: red"
          >
            {{ e }}
          </div>
          <div
            id="letra"
            role="alert"
            v-if="bd == true"
            style="width: 50%; height: 15px; color: green"
          >
            {{ ok }}
          </div>
          <button
            type="button"
            class="btn-close"
            data-bs-dismiss="offcanvas"
            aria-label="Close"
          ></button>
        </div>

        <div class="offcanvas-body small">
          <div class="row">
            <div class="col-sm-2">
              <label for=""> Nombre:</label>
              <input
                v-model="nombre"
                class="form-control"
                type="text"
                placeholder="Escriba el nombre..."
              />
            </div>
            <div class="col-sm-2">
              <label for="">Telefono:</label>
              <input
                v-model="telefono"
                class="form-control"
                type="number"
                placeholder="Escriba el nombre..."
              />
            </div>
            <div class="col-sm-2">
              <label for="">Mas Info....(opcional:)</label>
              <input
                v-model="opcion"
                class="form-control"
                type="text"
                placeholder="Escriba el nombre..."
              />
            </div>
            <div class="col-sm-2">
              <label for="">Estado:</label>
              <select
                v-model="estado"
                class="form-select"
                aria-label="Default select example"
              >
                <option value="1">Pagada</option>
                <option value="2">Disponible</option>
                <option value="3">En espera..</option>
              </select>
            </div>

            <div class="col-sm-4">
              <div class="d-grid gap-2 col-8 mx-auto">
                <button
                  id="sombra"
                  class="btn btn-success mt-4"
                  type="button"
                  @click="guardar()"
                >
                  comprar
                </button>
                <br />
              </div>
            </div>
          </div>
        </div>
      </div>

      <div
        class="modal fade"
        id="exampleModal"
        tabindex="-1"
        aria-labelledby="exampleModalLabel"
        aria-hidden="true"
      >
        <div
          class="modal-dialog modal-dialog-centered modal-dialog-scrollable modal-fullscreen"
        >
          <div class="modal-content">
            <div class="modal-header">
              <h1
                class="modal-title fs-5"
                id="exampleModalLabel"
                style="color: red"
              >
                <b>Informacion de las boletas</b>
              </h1>
              <button
                type="button"
                class="btn-close"
                data-bs-dismiss="modal"
                aria-label="Close"
              ></button>
            </div>
            <div class="modal-body">
              <div class="row">
                <div class="col-sm-12">
                  <table class="table table table-hover" id="clientesTable">
                    <thead>
                      <tr>
                        <th scope="col">Num</th>
                        <th scope="col">Nombre</th>
                        <th scope="col">Telefono</th>
                        <th scope="col">Opcional</th>
                        <th scope="col">Estado</th>
                      </tr>
                    </thead>
                    <tbody v-for="(c, i) in clientesFiltrados" :key="i">
                      <tr>
                        <td>{{ c.id }}</td>
                        <td>{{ c.nombre }}</td>
                        <td>{{ c.telefono }}</td>
                        <td>{{ c.opcion }}</td>
                        <td>
                          <span style="color: green" v-if="c.estado == 2"
                            >Disponible</span
                          >
                          <span style="color: red" v-else-if="c.estado == 1"
                            >Pagada</span
                          >
                          <span style="color: blue" v-else>En espera...</span>
                        </td>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </div>
            </div>

            <div class="modal-footer">
              <div class="container-fluid">
                <div class="row">
                  <div class="col-sm-1 mt-2">
                    <b>Seleccionar:</b>
                  </div>

                  <div class="col-sm-8">
                    <select
                      class="form-select form-select"
                      v-model="estadoFiltrado"
                    >
                      <option value="todos">Todas las boletas</option>
                      <option value="1" style="color: red">Pagadas</option>
                      <option value="2" style="color: green">
                        Disponibles
                      </option>
                      <option value="3" style="color: blue">
                        En espera...
                      </option>
                    </select>
                  </div>

                  <div class="col-sm-2">
                    <div class="d-grid gap-2">
                      <button
                        id="sombra"
                        class="btn btn-warning"
                        type="button"
                        @click="generatePDF()"
                      >
                        Descargar 📥
                      </button>
                    </div>
                  </div>

                  <div class="col-sm-1">
                    <div class="d-grid gap-2">
                      <button
                        id="sombra"
                        type="button"
                        class="btn btn-secondary"
                        data-bs-dismiss="modal"
                      >
                        Salir
                      </button>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import html2canvas from "html2canvas";
import jsPDF from "jspdf";
import { ref, computed } from "vue";

export default {
  setup() {
    let clientes = ref([
      { id: 0, nombre: "sucia", telefono: 2312313, opcion: "", estado: "1" },
      {
        id: 1,
        nombre: "daniel maluendas",
        telefono: 324324324,
        opcion: "que lo q",
        estado: "3",
      },
      { id: 2, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 3, nombre: "", telefono: 0, opcion: "", estado: "2" },
      {
        id: 4,
        nombre: "nithson",
        telefono: 123443223,
        opcion: "sisas apa",
        estado: "1",
      },
      {
        id: 5,
        nombre: "moises",
        telefono: 324234234,
        opcion: "careloco",
        estado: "3",
      },
      { id: 6, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 7, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 8, nombre: "", telefono: 0, opcion: "", estado: "2" },
      {
        id: 9,
        nombre: "camilo",
        telefono: 123213123,
        opcion: "qweqewqe",
        estado: "1",
      },
      { id: 10, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 11, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 12, nombre: "", telefono: 0, opcion: "", estado: "2" },
      {
        id: 13,
        nombre: "andres maluendas",
        telefono: 32424234,
        opcion: "",
        estado: "3",
      },
      { id: 14, nombre: "", telefono: 0, opcion: "", estado: "2" },
      {
        id: 15,
        nombre: "sergio armando",
        telefono: 234324324,
        opcion: "wwwww",
        estado: "3",
      },
      { id: 16, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 17, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 18, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 19, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 20, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 21, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 22, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 23, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 24, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 25, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 26, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 27, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 28, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 29, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 30, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 31, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 32, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 33, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 34, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 35, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 36, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 37, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 38, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 39, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 40, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 41, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 42, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 43, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 44, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 45, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 46, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 47, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 48, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 49, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 50, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 51, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 52, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 53, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 54, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 55, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 56, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 57, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 58, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 59, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 60, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 61, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 62, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 63, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 64, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 65, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 66, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 67, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 68, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 69, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 70, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 71, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 72, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 73, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 74, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 75, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 76, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 77, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 78, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 79, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 80, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 81, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 82, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 83, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 84, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 85, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 86, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 87, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 88, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 89, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 90, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 91, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 92, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 93, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 94, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 95, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 96, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 97, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 98, nombre: "", telefono: 0, opcion: "", estado: "2" },
      { id: 99, nombre: "", telefono: 0, opcion: "", estado: "2" },
    ]);

    let fl = ref(true);
    let bd = ref(false);
    let ok = ref("");
    let e = ref("");
    let nombre = ref("");
    let telefono = ref("");
    let opcion = ref("");
    let id = ref("");
    let cli = ref("");
    let estado = ref("");
    let estadoFiltrado = ref("todos");

    let clientesFiltrados = computed(() => {
      if (estadoFiltrado.value === "todos") {
        return clientes.value;
      } else {
        return clientes.value.filter((c) => c.estado === estadoFiltrado.value);
      }
    });

    function generatePDF() {
      let table = document.querySelector("#clientesTable");
      html2canvas(table).then((canvas) => {
        let pdf = new jsPDF();
        let imgData = canvas.toDataURL("image/png");
        let imgWidth = 210;
        let pageHeight = 295;
        let imgHeight = (canvas.height * imgWidth) / canvas.width;
        let heightLeft = imgHeight;
        let position = 0;

        pdf.addImage(imgData, "PNG", 0, position, imgWidth, imgHeight);
        heightLeft -= pageHeight;

        while (heightLeft > 0) {
          position = heightLeft - imgHeight;
          pdf.addPage();
          pdf.addImage(imgData, "PNG", 0, position, imgWidth, imgHeight);
          heightLeft -= pageHeight;
        }
        pdf.save("clientes.pdf");
      });
    }

    function guardar() {
      if (!nombre.value) {
        e.value = "Falta nombre";
        fl.value = false;
        setTimeout(() => {
          fl.value = true;
        }, 3000);
      } else if (!telefono.value) {
        e.value = "Falta telefono";
        fl.value = false;
        setTimeout(() => {
          fl.value = true;
        }, 3000);
      } else {
        ok.value = "Agregado Correctamente";
        bd.value = true;
        setTimeout(() => {
          bd.value = false;
        }, 3000);
        clientes.value[cli.value].nombre = nombre.value;
        clientes.value[cli.value].telefono = telefono.value;
        clientes.value[cli.value].opcion = opcion.value;
        clientes.value[cli.value].estado = estado.value;
        nombre.value = "";
        telefono.value = "";
        opcion.value = "";
        estado.value = "";
      }
    }
    function editar(c, i) {
      (nombre.value = c.nombre),
        (telefono.value = c.telefono),
        (opcion.value = c.opcion);
      estado.value = c.estado;
      cli.value = i;
      id.value = i;
      console.log(`estas editando ${i}`);
    }

    return {
      clientes,
      cli,
      nombre,
      id,
      telefono,
      guardar,
      editar,
      opcion,
      fl,
      bd,
      e,
      ok,
      estado,
      estadoFiltrado,
      clientesFiltrados,
      generatePDF,
    };
  },
};
</script>
