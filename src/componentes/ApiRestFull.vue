<template>

  <section class="src-componentes-api-rest-full">
      <div class="jumbotron">
        <h2>Formulario para el ingreso de gastos</h2>
        <hr>

        <!-- <pre>{{ usuarios }}</pre> -->


        <vue-form :state="formState" @submit.prevent="enviar(formData.nombre, formData.descripcion, formData.aPagar)">
            <!-- Campo nombre -->
            
            <validate tag="div">
                <label for="nombre">Nombre</label>
                  <input 
                  type="text" 
                  id="nombre" 
                  name="nombre"
                  class="form-control"
                  autocomplete="off"
                  v-model.trim="formData.nombre"
                  required
                  :minlength="nombreLengthMin"
                  :maxlength="nombreLengthMax"
                  no-espacios
                >
                <!-- Carteles de validación de campo -->
                
                <field-messages name="nombre" show="$dirty">
                <div slot="required" class="alert alert-danger mt-1">Campo requerido</div>
                <div slot="no-espacios" class="alert alert-danger mt-1">
                  No se permiten espacios intermedios en este campo
                </div>
                <div slot="minlength" class="alert alert-danger mt-1">
                  Se deben ingresar como mínimo {{nombreLengthMin}} caracteres
                </div>
                <div v-if="formData.nombre.length == nombreLengthMax" class="alert alert-danger mt-1">
                  Máximo de caracteres ({{nombreLengthMax}}) alcanzados
                </div>
              </field-messages>
            </validate>
            <br>

            <!-- Campo descripcion -->
              <validate tag="div">
              <label for="descripcion">Descripcion</label>
              <input 
                type="text" 
                id="descripcion" 
                class="form-control"
                name="descripcion"
                autocomplete="off"
                v-model.number="formData.descripcion"
                required
              >
              <!-- Carteles de validación de campo -->
              <field-messages name="descripcion" show="$dirty">
                <div slot="required" class="alert alert-danger mt-1">Campo requerido</div>
              </field-messages>
            </validate>
            <br>

            <!-- Campo monto a pagar -->
          <validate tag="div">
            <label for="aPagar">Importe</label>
            <input 
              type="number" 
              id="aPagar" 
              class="form-control"
              name="aPagar"
              autocomplete="off"
              v-model.trim="formData.aPagar"
              required
            >
            <!-- Carteles de validación de campo -->
            <field-messages name="aPagar" show="$dirty">
              <div slot="required" class="alert alert-danger mt-1">Campo requerido</div>
            </field-messages>
          </validate>


          <br>
        <button class="btn btn-success my-4" :disabled="formState.$invalid" type="submit">Enviar</button>
        </vue-form>
       <hr>

            <div v-if="usuarios.length">
       <h2>Detalle de gastos</h2>
       <label for="presupuesto">Presupuesto</label>
              <input 
                type="number" 
                id="presupuesto" 
                class="form-control"
                name="presupuesto"
                autocomplete="off"
                v-model.number="formData.presupuesto"
                required
              >
       <hr>

             <!-- tabla -->

              <div class="table-responsive">
                <table class="table">
                    <tr class="bg-dark text-white">
                        <th>NOMBRE</th>
                        <th>DESCRIPCION</th>
                        <th>IMPORTE</th>
                        <th>FECHA</th>
                    </tr>
                    
                    <tr class="table-secondary" v-for="(usuario, index) in usuarios" :key="index">
                        <td>{{ usuario.nombre }}</td>
                        <td>{{ usuario.descripcion }}</td>
                        <td>$ {{usuario.aPagar }}</td>
                        <td>{{ usuario.fechaDelRegistro }}</td>
                    </tr>
                    <tr :class="getColorFila(index)">
                        <td>{{ textoApagar }}</td>
                        <td> </td>
                        <td>$ {{ getTotalAPagar() }}</td>
                        <td> </td>
                  </tr>
                </table>
                <br>          
              </div>
              <h8>Nota: Si la fila está en verde es porque se gastó menos de 1000. Si está en celeste se gastó entre 1000 y 5000. Si está en amarillo se gastó mas de 5000. Si está en rojo se excedió el presupuesto</h8>

      </div>
      <h3 v-else class="alert alert-warning">
      Aun no se enviaron gastos
      </h3>
      </div>
  </section>

</template>

<script lang="js">
import moment  from "moment"; 

  export default  {
    name: 'src-componentes-api-rest-full',
    props: [],

//}

    /* --------------------------------------------------------------- */
    /*          CICLO DE VIDA DE LOS COMPONENTES DE VUE                */
    /* --------------------------------------------------------------- */
    /*  https://v3.vuejs.org/api/options-lifecycle-hooks.html#updated  */
    /* --------------------------------------------------------------- */

    /* --------------------------------------------------------------- */
    //Instanciación del componente
     beforeCreate() {
      console.log('ApiRestFull -> beforeCreate')
    },
    created() {
      console.log('ApiRestFull -> created')
    },
    /* --------------------------------------------------------------- */
    //Montaje del componente en la vista
    beforeMount() {
      console.log('ApiRestFull -> beforeMount')
    },
    mounted() {
      console.log('ApiRestFull -> mounted')
      this.getUsuarios()

    },
    /* --------------------------------------------------------------- */
    //Actualización de la vista del componente
   beforeUpdate() {
      console.log('ApiRestFull -> beforeUpdate')
    },
    updated() {
      console.log('ApiRestFull -> updated')
    },
    /* --------------------------------------------------------------- */
    //Destrucción del componente
    beforeDestroy() {
      console.log('ApiRestFull -> beforeDestroy')
    },
    destroyed() {
      console.log('ApiRestFull -> destroyed')

      clearInterval(this.refInterval)
    }, 
    /* --------------------------------------------------------------- */

    data () {
      return {
      usuarios : [
       /* {nombre: 'Juan', descripcion: "hola", aPagar: 123, fechaDelRegistro: '1/6/2021 12:27:20'},
        {nombre: 'Dario', descripcion: "hola", aPagar: 555, fechaDelRegistro: '6/5/2021 22:21:33'},
        {nombre: 'Fernando', descripcion: "hola", aPagar: 854, fechaDelRegistro: '2/6/2021 15:37:20'}*/
      ],
      refInterval : null,
      nombreLengthMin : 3,
      nombreLengthMax: 15,
      formData : this.getInicialData(),
      formState: {},
      textoApagar: "TOTAL A PAGAR"
      }
    },

    methods: {
      getInicialData() {
      return {
        nombre: '',
        descripcion: '',
        aPagar: '',
        fechaDelRegistro: '',
        presupuesto: ''
      }
    },
    getTotalAPagar(){
      let total=0
      //let ultNumero
      this.usuarios.forEach(element => {
        //ultNumero=element.aPagar
        total=Number(total) + Number (element.aPagar)
      });
      return total
    },

    getColorFila(i){ 
      let colorFila
      const total=this.getTotalAPagar(i)
      let costoPresupuesto = this.formData.presupuesto

      if(total>costoPresupuesto && costoPresupuesto!==''){
        colorFila="table-danger"
      } else if (total<1000) {
        colorFila="table-success"
      } else if (total>=1000 && total<5000){
        colorFila="table-primary"
      } else {
        colorFila="table-warning"
      }
      return colorFila
    },

      enviar(nom, d, pago) { 
        console.log({...this.formData})
        this.postUsuario(nom, d, pago)
        this.formData = this.getInicialData()
        this.formState._reset()
        
    },

      async getUsuarios() {
        console.log('getUsuarios')
      },

      async postUsuario(nom, d, pago) {
        console.log('postUsuario')
         let usuario = {
           nombre: nom,
           descripcion: d,
           aPagar: pago,
           fechaDelRegistro: moment().format('L') + " " + moment().format('LTS'),
         }
        this.usuarios.push(usuario)

      },
    },

    computed: {

    }
}


</script>

<style scoped lang="css">
  .src-componentes-api-rest-full {

  }

  .jumbotron {
    /* Permalink - use to edit and share this gradient: https://colorzilla.com/gradient-editor/#f9c667+0,f79621+100;Orange+3D+%234 */
    background: rgb(249,198,103); /* Old browsers */
    background: -moz-linear-gradient(left,  rgba(249,198,103,1) 0%, rgba(247,150,33,1) 100%); /* FF3.6-15 */
    background: -webkit-linear-gradient(left,  rgba(249,198,103,1) 0%,rgba(247,150,33,1) 100%); /* Chrome10-25,Safari5.1-6 */
    background: linear-gradient(to right,  rgba(249,198,103,1) 0%,rgba(247,150,33,1) 100%); /* W3C, IE10+, FF16+, Chrome26+, Opera12+, Safari7+ */
    filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#f9c667', endColorstr='#f79621',GradientType=1 ); /* IE6-9 */

    color: rgb(63, 13, 13);
  }

  hr {
    background-color: rgb(255, 255, 255);
  }
  pre {
    color: rgb(0, 0, 0);
}

</style>
