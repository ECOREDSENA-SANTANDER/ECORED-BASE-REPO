<template>
  <div>
    <div class="row align-items-center">
      <div class="d-none d-md-block col-3">
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
                {{ item.texto }}
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
        'Digite en el espacio indicado la palabra que dé sentido a cada frase.',
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
}
</script>

<style lang="sass" scoped>
img
  width: auto
  height: auto
  display: block
  margin: auto
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
