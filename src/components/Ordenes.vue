
<template>
  <div id="root">
    <h2>
      Orden
      <a
        class="btn-floating btn-small btn tooltipped -red"
        data-position="top"
        data-delay="50"
        id="boton"
        data-tooltip="Para modificar primero tienes que hacer click en  el boton de modificar en la tabla"
      >
        <i class="material-icons left">help</i>Update
      </a>
    </h2>
    <p>Pagina Actual: {{currentPage}}</p>
    <button v-on:click="anterior()" class="waves-effect waves-light btn-large">Anterior</button>
    <button v-on:click="siguiente()" class="waves-effect waves-light btn-large">Siguiente</button>
    <br />
    <table class="table centered">
      <thead>
        <tr>
          <th>idEmpleado</th>
          <th>idMesa</th>
          <th>idBebidas</th>
          <th>idProducto</th>
          <th>idCombos</th>
          <th>Modificar</th>
          <th>Borrar</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(orden, index) in data">
          <td>{{empleados2[index]}}</td>
          <td>{{mesas2[index]}}</td>
          <td>{{bebidas2[index]}}</td>
          <td>{{productos2[index]}}</td>
          <td>{{orden.idCombos}}</td>
          <td>
            <a
              v-on:click="startToModifyorden(orden)"
              class="btn-floating btn-small waves-effect waves-light green"
            >
              <i class="material-icons">update</i>
            </a>
          </td>
          <td>
            <a
              v-on:click="deleteorden(orden._id)"
              class="btn-floating btn-small waves-effect waves-light red"
            >
              <i class="material-icons">delete</i>
            </a>
          </td>
        </tr>
      </tbody>
    </table>
    <br />
    <ul id="tabs-swipe-demo" class="tabs">
      <li class="tab col s3">
        <a class="active" v-on:click="tabControl('test-swipe-1')" href="#test-swipe-1">Crear</a>
      </li>
      <li class="tab col s3">
        <a v-on:click="tabControl('test-swipe-2')" href="#test-swipe-2">Modificar</a>
      </li>
    </ul>

    <div class="input-field col s12">
      <input
        v-on:input="orden.idCombos = $event.target.value"
        type="text"
        v-model="orden.idCombos"
        :disabled="loading"
        id="idCombos"
      />
      <label for="idCombos">idCombos</label>
    </div>

    <div class="row">
      <!--Bebidas-->
      <div class="row -white" id="contenedorTablaExterna">
        <div class="col s6">
          <h5>Seleccionar ID Bebida:</h5>
          <p>(hacer click en el nombre deseado)</p>
          <hr />
          <ul v-for="bebida in bebidas">
            <li>
              <i class="material-icons left">pages</i>
              {{bebida.nombre}}
              <a
                v-on:click="newBebida(bebida._id)"
                class="btn-floating btn-small waves-effect waves-light black secondary-content"
              >
                <i class="material-icons">done</i>
              </a>
            </li>
          </ul>
        </div>
        <div id="importance" class="input-field col s6 center">
          <br />
          <label id="idBebida">
            <h4>
              <a v-on:click="borrarBebida()" class="waves-effect waves-light">
                <i class="material-icons">delete</i>
              </a>
              {{idBeb}} {{nombreBeb}}
            </h4>
          </label>
        </div>
      </div>
    </div>

    <div class="row">
      <!--Mesas-->
      <div class="row -white" id="contenedorTablaExterna">
        <div class="col s6">
          <h5>Seleccionar ID Mesa:</h5>
          <p>(hacer click en el nombre deseado)</p>
          <hr />
          <ul v-for="mesa in mesas">
            <li>
              <i class="material-icons left">pages</i>
              {{mesa.nombre}}
              <a
                v-on:click="newMesa(mesa._id)"
                class="btn-floating btn-small waves-effect waves-light black secondary-content"
              >
                <i class="material-icons">done</i>
              </a>
            </li>
          </ul>
        </div>
        <div id="importance" class="input-field col s6 center">
          <br />
          <label id="idBebida">
            <h4>
              <a v-on:click="borrarMesa()" class="waves-effect waves-light">
                <i class="material-icons">delete</i>
              </a>
              {{idMes}} {{nombreMes}}
            </h4>
          </label>
        </div>
      </div>
    </div>

    <div class="row">
      <!--Empleado-->
      <div class="row -white" id="contenedorTablaExterna">
        <div class="col s6">
          <h5>Seleccionar ID Empleado:</h5>
          <p>(hacer click en el nombre deseado)</p>
          <hr />
          <ul v-for="empleado in empleados">
            <li>
              <i class="material-icons left">pages</i>
              {{empleado.nombre}}
              <a
                v-on:click="newEmpleado(empleado._id)"
                class="btn-floating btn-small waves-effect waves-light black secondary-content"
              >
                <i class="material-icons">done</i>
              </a>
            </li>
          </ul>
        </div>
        <div id="importance" class="input-field col s6 center">
          <br />
          <label id="idBebida">
            <h4>
              <a v-on:click="borrarEmpleado()" class="waves-effect waves-light">
                <i class="material-icons">delete</i>
              </a>
              {{idEmp}} {{nombreEmp}}
            </h4>
          </label>
        </div>
      </div>
    </div>

    <div class="row">
      <!--Producto-->
      <div class="row -white" id="contenedorTablaExterna">
        <div class="col s6">
          <h5>Seleccionar ID Producto:</h5>
          <p>(hacer click en el nombre deseado)</p>
          <hr />
          <ul v-for="producto in productos">
            <li>
              <i class="material-icons left">pages</i>
              {{producto.nombre}}
              <a
                v-on:click="newProducto(producto._id)"
                class="btn-floating btn-small waves-effect waves-light black secondary-content"
              >
                <i class="material-icons">done</i>
              </a>
            </li>
          </ul>
        </div>
        <div id="importance" class="input-field col s6 center">
          <br />
          <label id="idBebida">
            <h4>
              <a v-on:click="borrarProducto()" class="waves-effect waves-light">
                <i class="material-icons">delete</i>
              </a>
              {{idPro}} {{nombrePro}}
            </h4>
          </label>
        </div>
      </div>
    </div>

    <div id="test-swipe-1" class="col s12">
      <a
        class="waves-effect waves-light btn-large"
        v-on:click="createorden"
        :disabled="loading"
        id="boton"
      >
        <i class="material-icons left">create</i>Crear
      </a>
    </div>
    <div id="test-swipe-2" class="col s12">
      <div
        class="card"
      >Atención: Los cambios realizados no se guardan hasta que haga click en el botón de update.</div>
      <a
        class="waves-effect waves-light btn-large"
        v-on:click="modifyorden"
        :disabled="loading"
        id="boton"
      >
        <i class="material-icons left">update</i>Update
      </a>
    </div>
  </div>
</template>

<script>
export default {
  name: "orden",
  data() {
    return {
      ordenes: [],
      orden: {},
      loading: false,
      idModificar: "",
      idBeb: "N/A",
      idMes: "N/A",
      idEmp: "N/A",
      idPro: "N/A",
      nombreBeb: "",
      nombreMes: "",
      nombreEmp: "",
      nombrePro: "",
      selectedTab: "test-swipe-1",
      bebida: {},
      bebidas: [],
      bebidas2: [],
      mesa: {},
      mesas: [],
      mesas2: [],
      empleado: {},
      empleados: [],
      empleados2: [],
      producto: {},
      productos: [],
      productos2: [],
      data: [],
      inicio: 0,
      final: 5,
      size: 1,
      currentPage: 1
    };
  },
  watch: {
    idBeb: function(val) {
      if (val != "N/A") {
        this.nombreBeb = "";
      } else {
        this.$http
          .get("http://localhost:8000/bebidas/searchbyid/{_id}")
          .then(response => {
            this.nombreBeb = response.body.bebida.nombre;
          });
      }
    },
    idMes: function(val) {
      if (val != "N/A") {
        this.nombreMes = "";
      } else {
        this.$http
          .get("http://localhost:8000/mesas/searchbyid/{_id}")
          .then(response => {
            this.nombreMes = response.body.mesa.nombre;
          });
      }
    },
    idEmp: function(val) {
      if (val != "N/A") {
        this.nombreEmp = "";
      } else {
        this.$http
          .get("http://localhost:8000/usuarios/searchbyid/{_id}")
          .then(response => {
            this.nombreEmp = response.body.empleado.nombre;
          });
      }
    },
    idPro: function(val) {
      if (val != "N/A") {
        this.nombrePro = "";
      } else {
        this.$http
          .get("http://localhost:8000/productos/searchbyid/{_id}")
          .then(response => {
            this.nombrePro = response.body.producto.nombre;
          });
      }
    }
  },

  methods: {
    siguiente() {
      if (this.currentPage < this.size) {
        this.currentPage = this.currentPage + 1;
        if (this.ordenes.length % 5 == 0) {
          this.inicio = this.inicio + 5;
          this.final = this.final + 5;
          this.data = this.ordenes.slice(this.inicio, this.final);
        } else if (
          this.ordenes.length % 5 != 0 &&
          this.currentPage == this.size
        ) {
          this.inicio = this.inicio + 5;
          this.final = this.final + (this.ordenes.length % 5);
          this.data = this.ordenes.slice(this.inicio, this.final);
        } else if (
          this.ordenes.length % 5 != 0 &&
          this.currentPage < this.size
        ) {
          this.inicio = this.inicio + 5;
          this.final = this.final + 5;
          this.data = this.ordenes.slice(this.inicio, this.final);
        }
      }
    },
    anterior() {
      if (this.currentPage > 1) {
        this.currentPage = this.currentPage - 1;
        if (this.currentPage < this.size) {
          if (this.ordenes.length % 5 == 0) {
            this.inicio = this.inicio - 5;
            this.final = this.final - 5;
            this.data = this.ordenes.slice(this.inicio, this.final);
          } else if (
            this.ordenes.length % 5 != 0 &&
            this.currentPage == this.size - 1
          ) {
            this.inicio = this.inicio - 5;
            this.final = this.final - (this.ordenes.length % 5);
            this.data = this.ordenes.slice(this.inicio, this.final);
          } else if (
            this.ordenes.length % 5 != 0 &&
            this.currentPage < this.size
          ) {
            this.inicio = this.inicio - 5;
            this.final = this.final - 5;
            this.data = this.ordenes.slice(this.inicio, this.final);
          }
        }
      }
    },
    getorden() {
      let _this = this;
      this.$http.get("http://localhost:8000/ordenes").then(response => {
        this.ordenes = response.body;
        response.body.map(function(value, key) {
          var i;
          var p = _this.bebidas;
          for (i = 0; i < p.length; i++) {
            if (value.idBebidas == p[i]._id) {
              _this.bebidas2.push(p[i].nombre);
            }
          }

          var j;
          var p1 = _this.mesas;
          for (j = 0; j < p1.length; j++) {
            if (value.idMesa == p1[j]._id) {
              _this.mesas2.push(p1[j].nombre);
            }
          }

          var k;
          var p2 = _this.empleados;
          for (k = 0; k < p2.length; k++) {
            if (value.idEmpleado == p2[k]._id) {
              _this.empleados2.push(p2[k].nombre);
            }
          }

          var l;
          var p3 = _this.productos;
          for (l = 0; l < p3.length; l++) {
            if (value.idProductos == p3[l]._id) {
              _this.productos2.push(p3[l].nombre);
            }
          }
        });
        this.data = this.ordenes.slice(this.inicio, this.final);
        if (this.ordenes.length % 5 == 0) {
          this.size = this.ordenes.length / 5;
        } else {
          this.size = parseInt(this.ordenes.length / 5) + 1;
        }
      });
    },
    newBebida(bebida_id) {
      this.idBeb = bebida_id;
    },
    borrarBebida() {
      this.idBeb = "N/A";
    },
    newMesa(mesa_id) {
      this.idMes = mesa_id;
    },
    borrarMesa() {
      this.idMes = "N/A";
    },
    newEmpleado(empleado_id) {
      this.idEmp = empleado_id;
    },
    borrarEmpleado() {
      this.idEmp = "N/A";
    },
    newProducto(producto_id) {
      this.idPro = producto_id;
    },
    borrarProducto() {
      this.idPro = "N/A";
    },
    createorden() {
      this.inicio = 0;
      this.final = 5;
      this.currentPage = 1;
      this.loading = true;

      this.orden.idBebidas = this.idBeb;
      this.orden.idMesa = this.idMes;
      this.orden.idEmpleado = this.idEmp;
      this.orden.idProductos = this.idPro;
      console.log("hkjdshfdshjfhsdfhj")
      if((this.orden.idCombos == undefined)  ||  (this.orden.idCombos.length < 4) || (this.idMes == "N/A") || (this.idEmp== "N/A")){
        this.loading = false;
        sweetAlert("Oops...", "Error al crear, datos invalidos", "error");
      } else {
        this.$http
        .post("http://localhost:8000/ordenes/create", this.orden)
        .then(response => {
          this.loading = false;
          if (response.body.success) {
            this.orden = {};
            sweetAlert(
              "Creado con exito!",
              "Los cambios estan en la tabla",
              "success"
            );
            this.getorden();
          } else {
            sweetAlert("Oops...", "Error al crear", "error");
          }
        });
      }

      
    },
    tabControl(idTab) {
      if (idTab === "test-swipe-1") {
        this.idModificar = "";
        this.selectedTab = "test-swipe-1";
        this.orden = {};
      } else {
        if (this.idModificar === "") {
          swal(
            "Recuerda!",
            "Para modificar primero tienes que hacer click en  el boton de modificar en la tabla"
          );
        }
      }
    },
    startToModifyorden(orden) {
      this.selectedTab = "test-swipe-2";
      this.idModificar = orden._id;
      this.orden = orden;
      this.idBeb = orden.idBebida;
      $("ul.tabs").tabs("select_tab", "test-swipe-2");
      Materialize.updateTextFields();
    },
    modifyorden() {
      this.loading = true;
      if (this.idModificar != "") {
        Materialize.updateTextFields();
        this.orden.idBebida = this.idBeb;
        this.$http
          .put(
            "http://localhost:8000/ordenes/update/" + this.idModificar,
            this.orden
          )
          .then(response => {
            if (response.body.success) {
              this.getorden();
              this.loading = false;
              sweetAlert("Oops...", "Error al modificar", "error");
            } else {
              sweetAlert(
                "Modificado con exito!",
                "Los cambios estan en la tabla",
                "success"
              );
              this.orden = {};
              this.loading = false;
            }
          });
      }
    },
    deleteorden(idOrden) {
      let _this = this;
      sweetAlert(
        {
          title: "¿Estás seguro?",
          text: "No podrás revertir los cambios",
          type: "warning",
          showCancelButton: true,
          confirmButtonText: "Eliminar",
          cancelButtonText: "Cancelar",
          showCloseButton: true,
          showLoaderOnConfirm: true
        },
        function(inputValue) {
          setTimeout(function() {
            if (inputValue) {
              //****************************************************** */
              _this.loading = true;
              _this.$http
                .delete("http://localhost:8000/ordenes/delete/" + idOrden)
                .then(response => {
                  this.loading = false;
                  if (response.body.success) {
                    sweetAlert("Oops...", "Error al eliminar", "error");
                    _this.getorden();
                  } else {
                    sweetAlert(
                      "Deleted!",
                      "Los cambios estan en la tabla",
                      "success"
                    );
                    _this.inicio = 0;
                    _this.final = 5;
                    _this.currentPage = 1;
                    _this.getorden();
                  }
                });
              //****************************************************** */
            } else {
              sweetAlert("Cancelado", "Tus datos están a salvo", "info");
            }
          }, 500);
        }
      );
    },
    getBebidas() {
      this.$http.get("http://localhost:8000/bebidas").then(response => {
        console.log(response);
        this.bebidas = response.body;
      });
    },
    getMesas() {
      this.$http.get("http://localhost:8000/mesas").then(response => {
        console.log(response);
        this.mesas = response.body;
      });
    },
    getEmpleados() {
      this.$http.get("http://localhost:8000/usuarios").then(response => {
        console.log(response);
        this.empleados = response.body;
      });
    },
    getProductos() {
      this.$http.get("http://localhost:8000/productos").then(response => {
        console.log(response);
        this.productos = response.body;
      });
    }
  },

  beforeMount() {
    this.getBebidas();
    this.getProductos();
    this.getMesas();
    this.getEmpleados();
    this.getorden();
  },
  mounted() {
    $("ul.tabs").tabs();
    $("select").material_select();
    $(".tooltipped").tooltip({ delay: 50 });
  }
};
</script>

<style scoped>
#importance {
  color: black !important;
  opacity: 0.7;
  text-align: center;
  font-family: "Roboto", sans-serif !important;
  font-weight: lighter;
}
.collection-item {
  color: black;
}
.card {
  padding: 10px;
  font-family: "Roboto", sans-serif;
  font-weight: lighter;
  background-color: black;
  font-size: 15px;
  color: white;
}
#contenedorTablaExterna {
  border-radius: 5px;
}
td {
  font-family: "Source Sans Pro", sans-serif;
}
th {
  color: white;
  background: #5994aa;
  border-bottom: 4px solid #9ea7af;
  border-right: 1px solid #343a45;
  font-size: 25px;
  font-weight: 100;
  padding: 24px;
  text-align: left;
  text-shadow: 0 1px 1px rgba(0, 0, 0, 0.1);
  vertical-align: middle;
}
.table thead {
  font-family: "Roboto", sans-serif;
  font-weight: bold;
  font-size: 30px;
}
.table {
  color: black;
  font-family: "Spectral", serif;
  font-size: 20px;
  background: white;
  border-radius: 3px;
  border-collapse: collapse;
  height: 320px;
  padding: 5px;
  width: 100%;
  box-shadow: 0 5px 10px rgba(0, 0, 0, 0.1);
  animation: float 5s infinite;
}

#homeCard {
  height: 230px;
  display: -webkit-box;
  -webkit-box-pack: center;
  -webkit-box-align: center;
  text-align: center;
  font-family: "Roboto", sans-serif;
  font-size: 20px;
  color: #06152f;
}
.-white {
  background-color: #f4f0ea;
  color: black;
}
.-lightblue {
  background-color: #5994aa;
  color: #fff;
}
.-blue {
  background-color: #06152f;
  color: #fff;
}
.-red {
  background-color: #ff0b00;
  color: #fff;
}
.-black {
  background-color: #262626;
  color: #fff;
}
#TOOLTIP-ID.backdrop {
  background-color: red;
}
h4 {
  text-align: center;
  color: black !important;
}
#root {
  font-family: "Playfair Display";
  font-weight: bold;
  color: white;
}

#test-swipe-1 {
  /*background-color: #E6E2AF;*/
  color: #4c1b1b;
  padding-left: 50px;
  padding-right: 50px;
  padding-top: 20px;
  padding-bottom: 30px;
  text-align: center;
}

#test-swipe-2 {
  /*background-color: #F6E497;*/
  color: #4c1b1b;
  padding-left: 50px;
  padding-right: 50px;
  padding-top: 20px;
  padding-bottom: 30px;
  text-align: center;
}

.tabs .indicator {
  background-color: #a7a37e !important;
  color: #4c1b1b !important;
}

.tabs {
  background-color: #ff0b00 !important;
  color: #4c1b1b !important;
  font-family: "Spectral", serif;
  font-weight: bold;

  border-radius: 3px;
}

.tabs .tab a.active {
  color: white;
}

.tabs .tab a:hover {
  color: gray;
}

.tabs .tab a {
  color: #4c1b1b;
}

#boton {
  background-color: #5994aa;
}

/* label focus color */
input {
  font-family: "Roboto", sans-serif;
  color: white;
  font-weight: normal;
  font-size: 17px;
}
textarea {
  font-family: "Roboto", sans-serif;
  color: white;
  font-weight: normal;
  font-size: 17px;
}
label {
  font-size: 17px;
  font-family: "Roboto", sans-serif;
  font-weight: normal;
}
.input-field input:focus + label {
  color: #5994aa !important;
}
/* label underline focus color */

.row .input-field input:focus {
  border-bottom: 1px solid #5994aa !important;
  box-shadow: 0 1px 0 0 #5994aa !important;
}

.input-field input:focus + label {
  color: #5994aa !important;
}
/* label underline focus color */

.row .input-field input:focus {
  border-bottom: 1px solid #5994aa !important;
  box-shadow: 0 1px 0 0 #5994aa !important;
}

.input-field textarea:focus + label {
  color: #5994aa !important;
}
/* label underline focus color */

.row .input-field textarea:focus {
  border-bottom: 1px solid #5994aa !important;
  box-shadow: 0 1px 0 0 #5994aa !important;
}
</style>


