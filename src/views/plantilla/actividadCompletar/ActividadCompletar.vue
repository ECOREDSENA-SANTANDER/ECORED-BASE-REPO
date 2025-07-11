<template>
  <div class="tarjeta--blanca">
    <div class="row align-items-center mb-4">
      <div class="col-auto">
        <img src="@/assets/componentes/icon-actividad.svg" alt="" />
      </div>
      <div class="col ">
        <h2 class="titulo-tercero mb-0">
          {{ parrafo.titulo }}
        </h2>
        <p class="mb-0" v-html="parrafo.introduccion"></p>
      </div>
    </div>
    <div class="tarjeta tarjeta--lightest-gray px-4 pb-4 pt-4 px-md-5">
      <ActividadResultados
        v-if="mostrarResultados"
        :respuestas="resultadosVerificacion"
        :mensaje-aprobado="parrafo.mensaje_final_aprobado"
        :mensaje-reprobado="parrafo.mensaje_final_reprobado"
        :titulo-aprobado="parrafo.titulo_aprobado"
        :titulo-reprobado="parrafo.titulo_reprobado"
        :porcentaje-aprobadas="porcentajeAprobacion"
        :total-preguntas-base="parrafo.textos.length"
        :preguntas-count="parrafo.textos.length"
        @reiniciar="reiniciarActividad"
      />
      <ActividadParrafo
        v-else
        :instruccion="parrafo.instruccion"
        :imagen="parrafo.imagen"
        :textos="parrafo.textos"
        @respuesta-cambiada="actualizarRespuesta"
      />
    </div>
    <ActividadCompletarFooter
      v-if="!mostrarResultados"
      class="mx-4 mx-md-5"
      @continuar="verificarRespuestas"
    />
  </div>
</template>

<script>
import ActividadParrafo from './ActividadParrafo'
import ActividadCompletarFooter from './ActividadCompletarFooter'
import ActividadResultados from './ActividadResultados'

export default {
  name: 'ActividadCompletar',
  components: {
    ActividadParrafo,
    ActividadCompletarFooter,
    ActividadResultados,
  },
  props: {
    parrafo: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      respuestasUsuario: {},
      mostrarResultados: false,
      resultadosVerificacion: [],
      porcentajeAprobacion: 0,
    }
  },
  mounted() {
    this.inicializarRespuestas()
  },
  methods: {
    inicializarRespuestas() {
      // Inicializar respuestas vacías para cada texto
      this.parrafo.textos.forEach(item => {
        this.$set(this.respuestasUsuario, item.id, '')
      })
    },

    actualizarRespuesta(data) {
      this.respuestasUsuario = { ...data.todasLasRespuestas }
    },

    verificarRespuestas() {
      const resultados = []
      let correctas = 0

      this.parrafo.textos.forEach(item => {
        const respuestaUsuario = (this.respuestasUsuario[item.id] || '')
          .trim()
          .toLowerCase()
        const respuestaCorrecta = item.respuesta.toLowerCase()
        const esCorrecta = respuestaUsuario === respuestaCorrecta

        if (esCorrecta) {
          correctas++
        }

        resultados.push({
          id: item.id,
          pregunta: item.texto,
          respuestaUsuario: this.respuestasUsuario[item.id] || '',
          respuestaCorrecta: item.respuesta,
          esCorrecta: esCorrecta,
        })
      })

      this.resultadosVerificacion = resultados
      this.porcentajeAprobacion = (correctas / this.parrafo.textos.length) * 100
      this.mostrarResultados = true
    },

    reiniciarActividad() {
      this.mostrarResultados = false
      this.inicializarRespuestas()
      this.resultadosVerificacion = []
      this.porcentajeAprobacion = 0

      // Limpiar los inputs en el DOM
      this.$nextTick(() => {
        const inputs = this.$el.querySelectorAll('.input-inline')
        inputs.forEach(input => {
          input.value = ''
        })
      })
    },
  },
}
</script>

<style lang="sass" scoped>
.boton--disabled
  opacity: 0.5
  pointer-events: none

.tarjeta--lightest-gray
  border: 3px solid #dce4eb
</style>
