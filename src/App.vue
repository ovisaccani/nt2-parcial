<template>
  <div id="app" class="container-fluid mt-3">
    <div class="jumbotron">
      <h1>Examen Parcial de Nuevas Tecnologías 2</h1>
      <br>

      <Navbar />
      <router-view></router-view>

      <!-- <Binding />
      <Estructura />
      <Atributos />
      <Contadores /> -->

    </div>
  </div>
</template>

<script>
/* import Binding from './componentes/Binding.vue'
import Estructura from './componentes/Estructura.vue'
import Atributos from './componentes/Atributos.vue'
import Contadores from './componentes/Contadores.vue' */
import Navbar from './componentes/Navbar.vue'


export default {
  name: 'App',
  components: {
    /* Binding,
    Estructura,
    Atributos,
    Contadores, */
    Navbar
  }
}
</script>

<style>
  #app {
  }

  h1 {
    color: rgb(136, 0, 68);
  }

  .jumbotron {
    padding-top: 48px !important;
    padding-bottom: 48px !important;
  }
</style>
