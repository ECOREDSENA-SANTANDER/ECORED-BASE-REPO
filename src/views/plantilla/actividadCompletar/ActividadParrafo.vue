<template>
  <div>
    <div class="row align-items-center">
      <div class="d-none d-lg-block col-3">
        <img :src="imagen" alt="" />
      </div>
      <div class="col">
        <div>
          <h3 class="mb-4 pb-4">
            {{ instruccion }}
          </h3>
          <div class="row align-items-center">
            <ol class="lista-ol--cuadro lista-ol--separador">
              <li v-for="(item, index) in textos" :key="item.id" class="d-flex">
                <div class="lista-ol--cuadro__vineta v--green">
                  <span>{{ index + 1 }}</span>
                </div>
                <div
                  class="texto-con-input"
                  v-html="procesarTextoConInput(item.texto, item.id)"
                ></div>
              </li>
            </ol>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ActividadParrafo',
  props: {
    instruccion: {
      type: String,
      default:
        'Digite en el espacio indicado la palabra que dé sentido a cada frase.',
    },
    imagen: {
      type: String,
      default: require('@/assets/actividad/imagen1.png'),
    },
    textos: {
      type: Array,
      required: true,
      default: () => [],
    },
  },
  data() {
    return {
      respuestasUsuario: {},
    }
  },
  mounted() {
    // Inicializar respuestas del usuario
    this.textos.forEach(item => {
      this.$set(this.respuestasUsuario, item.id, '')
    })

    // Agregar event listeners para los inputs después de que se rendericen
    this.$nextTick(() => {
      this.agregarEventListeners()
    })
  },
  methods: {
    procesarTextoConInput(texto, itemId) {
      // Reemplazar [respuesta] con un input envuelto en un span para posicionamiento
      return texto.replace(
        /\[respuesta\]/g,
        `<span class="input-container"><input type="text" class="input-inline" data-item-id="${itemId}" placeholder="[?]" /></span>`,
      )
    },

    agregarEventListeners() {
      // Agregar event listeners a todos los inputs
      const inputs = this.$el.querySelectorAll('.input-inline')
      inputs.forEach(input => {
        input.addEventListener('input', this.manejarCambioInput)
      })
    },

    manejarCambioInput(event) {
      const itemId = parseInt(event.target.dataset.itemId)
      const valor = event.target.value
      this.$set(this.respuestasUsuario, itemId, valor)

      // Emitir evento para el componente padre
      this.$emit('respuesta-cambiada', {
        itemId: itemId,
        valor: valor,
        todasLasRespuestas: this.respuestasUsuario,
      })
    },
  },
}
</script>

<style lang="sass" scoped>
img
  width: auto
  height: auto
  display: block
  margin: auto

.texto-con-input
  flex: 1
  line-height: 1.6

// Contenedor para input e icono
::v-deep .input-container
  display: inline-block
  position: relative
  white-space: nowrap

// Estilos para el input inline
::v-deep .input-inline
  display: inline !important
  border: 2px solid #ddd !important
  background-color: #f5d982 !important
  padding: 4px 8px !important
  margin: 0 2px !important
  font-size: inherit !important
  font-family: inherit !important
  line-height: 1.2 !important
  min-width: 100px !important
  max-width: 150px !important
  text-align: center !important
  border-radius: 4px !important
  transition: all 0.3s ease !important
  vertical-align: baseline !important
  height: auto !important
  box-sizing: border-box !important

  &:focus
    outline: none !important
    border-color: #556a82 !important
    background-color: #f0d050 !important
    box-shadow: 0 0 0 2px rgba(85, 106, 130, 0.2) !important

  &::placeholder
    color: #666 !important
    font-size: inherit !important
    font-weight: normal !important

  // Estados de validación
  &.input-correcto
    border-color: #28a745 !important
    background-color: #d4edda !important
    color: #155724 !important

    &:focus
      border-color: #28a745 !important
      background-color: #c3e6cb !important
      box-shadow: 0 0 0 2px rgba(40, 167, 69, 0.25) !important

  &.input-incorrecto
    border-color: #dc3545 !important
    background-color: #f8d7da !important
    color: #721c24 !important

    &:focus
      border-color: #dc3545 !important
      background-color: #f1b0b7 !important
      box-shadow: 0 0 0 2px rgba(220, 53, 69, 0.25) !important

  // Estado deshabilitado
  &:disabled
    opacity: 0.8 !important
    cursor: not-allowed !important

.tarjeta--pregunta
  background: #dce4eb

.tarjeta-respuesta
  border: 2px solid transparent
  cursor: pointer
  transition: border-color 0.2s

  &:hover
    border-color: #556a82

    .tarjeta-respuesta__icon:not(.tarjeta-respuesta__icon--correcto):not(.tarjeta-respuesta__icon--incorrecto)
      background-image: url('~@/assets/actividad/vacio-hover.svg')

  &--correcta
    border-color: #61ca92ff
    background-color: #ebfff0ff

    &:hover
      border-color: #61ca92ff

  &--incorrecta
    border-color: #ff6b6bff
    background-color: #ffedecff

    &:hover
      border-color: #ff6b6bff

  &--disabled
    cursor: not-allowed
    pointer-events: none

  &__icon
    width: 32px
    height: 32px
    position: relative
    background-image: url('~@/assets/actividad/vacio.svg')

    &:hover
      background-image: url('~@/assets/actividad/vacio-hover.svg')

    &--correcto
      background-image: url('~@/assets/actividad/correcto.svg')

    &--incorrecto
      background-image: url('~@/assets/actividad/incorrecto.svg')
</style>
