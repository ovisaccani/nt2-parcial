<template>

  <section class="src-componentes-multipleChoice">
    <div class="jumbotron">
        <h2>Multiple Choice</h2>
        <div>
            <hr>
            <div class="table-responsive">
                <table class="table">
                    <tr class="bg-success text-white">
                        <th>Numero de Pregunta</th>
                        <th>Respuesta</th>
                    </tr>
                    <tr class="bg-info text-white" v-for="(examen, index) in examenes" :key="index">
                        <td>{{ examen.pregunta }}</td>
                        <td>{{ examen.respuesta }}</td>
                    </tr>
                </table>                
            </div>
            <hr>
        </div>
    </div>
  </section>

</template>

<script lang="js">

  export default  {
    name: 'src-componentes-estructura',
    props: [],
    mounted () {

    },
    data () {
      return {
        examenes : [
            { pregunta: '1',  respuesta: 'b, d',},
            { pregunta: '2',  respuesta: 'd'},
            { pregunta: '3',  respuesta: 'e'},
        ],
        nombre: '',
      }
    },
    methods: {
    },
    computed: {
    }
}


</script>

<style scoped lang="css">
  .src-componentes-estructura {

  }

  .jumbotron {
    background-color: pink;
    color: brown;
  }

  hr {
    background-color: #444;
  }

</style>
