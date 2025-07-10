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
      <ActividadResultados />
      <ActividadParrafo
        :instruccion="parrafo.instruccion"
        :imagen="parrafo.imagen"
      />
    </div>
    <ActividadCompletarFooter class="mx-4 mx-md-5" />
  </div>
</template>

<script>
import ActividadParrafo from './ActividadParrafo'
import ActividadCompletarFooter from './ActividadCompletarFooter'
export default {
  name: 'ActividadCompletar',
  components: {
    ActividadParrafo,
    ActividadCompletarFooter,
  },
  props: {
    parrafo: {
      type: Object,
      required: true,
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
