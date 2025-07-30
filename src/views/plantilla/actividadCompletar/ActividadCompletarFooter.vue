<template>
  <div class="tarjeta-avance">
    <div class="row align-items-end justify-content-end">
      <div class="col-auto">
        <button
          class="boton btn-lg boton--b py-3 px-4"
          :disabled="verificando"
          @click="$emit('continuar')"
        >
          <span v-if="!verificando">Verificar respuestas</span>
          <span v-else>Mostrando resultados en {{ contador }}s</span>
          <i v-if="!verificando" class="fas fa-arrow-right"></i>
          <i v-else class="fas fa-clock"></i>
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ActividadCompletarFooter',
  props: {
    verificando: {
      type: Boolean,
      default: false,
    },
    contador: {
      type: Number,
      default: 0,
    },
  },
}
</script>

<style lang="sass" scoped>
.tarjeta-avance
  background-color: #dce4ebff
  border-bottom-right-radius: 10px
  border-bottom-left-radius: 10px
  overflow: hidden
  @media (max-width: 560px)
    padding-left: .85rem !important

  .boton
    border-radius: 0

  .boton--disabled
    opacity: 0.5
    pointer-events: none

  &__barra-container
    background-color: #b4cce3ff
    border-radius: 8px
    height: 16px
    overflow: hidden
    .tarjeta-avance__barra
      background-color: #2c70abff
      height: 100%
      border-radius: 8px
      width: 0%
      transition: width 0.5s
</style>
