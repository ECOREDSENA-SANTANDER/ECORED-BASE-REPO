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
      :verificando="verificando"
      :contador="contadorResultados"
      @continuar="verificarRespuestas"
    />
  </div>
</template>

<script>
import ActividadParrafo from './ActividadParrafo'
import ActividadCompletarFooter from './ActividadCompletarFooter'
import ActividadResultados from './ActividadResultados'
import endGameSuccessSound from '@/assets/actividad/audio/end-game-success.mp3'
import endGameFailSound from '@/assets/actividad/audio/end-game-fail.mp3'

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
      verificando: false,
      contadorResultados: 0,
      intervaloContador: null,
    }
  },
  mounted() {
    this.inicializarRespuestas()
  },
  beforeDestroy() {
    // Limpiar intervalo si existe
    if (this.intervaloContador) {
      clearInterval(this.intervaloContador)
    }
  },
  methods: {
    inicializarRespuestas() {
      if (this.parrafo.textos.length > 0 && this.parrafo?.barajarPreguntas) {
        this.parrafo.textos.sort(() => Math.random() - 0.5)
      }
      this.parrafo.textos.forEach(item => {
        this.$set(this.respuestasUsuario, item.id, '')
      })
    },

    actualizarRespuesta(data) {
      this.respuestasUsuario = { ...data.todasLasRespuestas }
    },

    // Nueva función para normalizar texto removiendo tildes y caracteres especiales
    normalizarTexto(texto) {
      return texto
        .toLowerCase()
        .trim()
        .normalize('NFD')
        .replace(/[\u0300-\u036f]/g, '') // Remover diacríticos (tildes, acentos)
        .replace(/[^a-z0-9]/g, '') // Remover caracteres especiales, solo alfanuméricos
    },

    // REPRODUCIR SONIDOS
    reproducirSonido(audioSrc) {
      const audio = new Audio(audioSrc)
      audio.play()
    },

    verificarRespuestas() {
      if (this.verificando) return // Evitar múltiples clics

      const resultados = []
      let correctas = 0

      this.parrafo.textos.forEach(item => {
        const respuestaUsuario = this.respuestasUsuario[item.id] || ''
        const respuestaCorrecta = item.respuesta

        // Normalizar ambas respuestas para comparación sin tildes
        const respuestaUsuarioNormalizada = this.normalizarTexto(
          respuestaUsuario,
        )
        const respuestaCorrectaNormalizada = this.normalizarTexto(
          respuestaCorrecta,
        )

        const esCorrecta =
          respuestaUsuarioNormalizada === respuestaCorrectaNormalizada

        if (esCorrecta) {
          correctas++
        }

        // Aplicar feedback visual con icono
        this.aplicarFeedbackConIcono(item.id, esCorrecta)

        resultados.push({
          id: item.id,
          pregunta: item.texto,
          respuestaUsuario: respuestaUsuario,
          respuestaCorrecta: respuestaCorrecta,
          esCorrecta: esCorrecta,
        })
      })

      this.resultadosVerificacion = resultados
      this.porcentajeAprobacion = (correctas / this.parrafo.textos.length) * 100

      // Iniciar contador de 10 segundos
      this.iniciarContadorResultados()
    },

    aplicarFeedbackConIcono(itemId, esCorrecta) {
      const input = this.$el.querySelector(`input[data-item-id="${itemId}"]`)
      if (input) {
        // Remover clases anteriores
        input.classList.remove('input-correcto', 'input-incorrecto')

        // Agregar clase según resultado
        input.classList.add(esCorrecta ? 'input-correcto' : 'input-incorrecto')

        // Deshabilitar input para evitar cambios
        input.disabled = true

        // Crear y agregar icono
        this.agregarIconoFeedback(input, esCorrecta)
      }
    },

    agregarIconoFeedback(input, esCorrecta) {
      // Verificar si ya existe un icono para evitar duplicados
      const iconoExistente = input.parentNode.querySelector('.feedback-icon')
      if (iconoExistente) {
        iconoExistente.remove()
      }

      // Crear elemento del icono
      const icono = document.createElement('span')
      icono.className = `feedback-icon ${
        esCorrecta ? 'feedback-icon--correcto' : 'feedback-icon--incorrecto'
      }`

      // Insertar el icono después del input
      input.parentNode.insertBefore(icono, input.nextSibling)
    },

    iniciarContadorResultados() {
      this.verificando = true
      this.contadorResultados = 1 //

      this.intervaloContador = setInterval(() => {
        this.contadorResultados--

        if (this.contadorResultados <= 0) {
          clearInterval(this.intervaloContador)
          this.mostrarResultados = true
          this.verificando = false

          // REPRODUCIR SONIDO SEGUN RESULTADO
          //-------
          if (this.porcentajeAprobacion >= 70) {
            this.reproducirSonido(endGameSuccessSound)
          } else {
            this.reproducirSonido(endGameFailSound)
          }
        }
      }, 1000)
    },

    reiniciarActividad() {
      // Limpiar intervalo si existe
      if (this.intervaloContador) {
        clearInterval(this.intervaloContador)
      }

      this.mostrarResultados = false
      this.verificando = false
      this.contadorResultados = 0
      this.inicializarRespuestas()
      this.resultadosVerificacion = []
      this.porcentajeAprobacion = 0

      // Limpiar los inputs en el DOM
      this.$nextTick(() => {
        const inputs = this.$el.querySelectorAll('.input-inline')
        inputs.forEach(input => {
          input.value = ''
          input.disabled = false
          input.classList.remove('input-correcto', 'input-incorrecto')
        })

        // Remover todos los iconos de feedback
        const iconos = this.$el.querySelectorAll('.feedback-icon')
        iconos.forEach(icono => {
          icono.remove()
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

// Estilos para los iconos de feedback
::v-deep .feedback-icon
  display: inline-block
  width: 20px
  height: 20px
  margin-right: 8px
  background-size: contain
  background-repeat: no-repeat
  background-position: center
  vertical-align: middle
  animation: fadeInScale 0.3s ease-out

  &--correcto
    background-image: url('~@/assets/actividad/correcto.svg')

  &--incorrecto
    background-image: url('~@/assets/actividad/incorrecto.svg')

// Animación para la aparición del icono
@keyframes fadeInScale
  0%
    opacity: 0
    transform: scale(0.5)
  100%
    opacity: 1
    transform: scale(1)
</style>
