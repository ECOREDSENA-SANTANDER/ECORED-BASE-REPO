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
              <li>
                <div class="lista-ol--cuadro__vineta v--green">
                  <span>1</span>
                </div>
                En el ámbito de la informática, _________ información consiste
                en ocultarla, de forma que solo pueda interpretarse si se
                dispone de una clave o un código.
              </li>
              <li>
                <div class="lista-ol--cuadro__vineta v--green">
                  <span>2</span>
                </div>
                Los _________ son los equipos que forman parte de la red
                Blockchain, encargados de almacenar y distribuir en tiempo real
                copias actualizadas de las operaciones que se realizan.
              </li>
              <li>
                <div class="lista-ol--cuadro__vineta v--green">
                  <span>3</span>
                </div>
                Tradicionalmente, _________ es el nombre con el que se llama a
                las piezas que, adquiridas a cambio de dinero, sirve para
                recibir un bien o servicio.
              </li>
              <li>
                <div class="lista-ol--cuadro__vineta v--green">
                  <span>4</span>
                </div>
                _________ es la base de datos transaccional distribuida, formada
                por cadenas de bloques diseñadas para evitar su modificación una
                vez que un dato ha sido publicado.
              </li>
              <li>
                <div class="lista-ol--cuadro__vineta v--green">
                  <span>4</span>
                </div>
                Se puede gozar de total _________, dependiendo de la necesidad
                puntual de cada aplicación.
              </li>
              <li>
                <div class="lista-ol--cuadro__vineta v--green">
                  <span>5</span>
                </div>
                La _________ es una técnica destinada a alterar las
                representaciones lingüísticas de ciertos mensajes con el fin de
                hacerlos ininteligibles a receptores no autorizados.
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
