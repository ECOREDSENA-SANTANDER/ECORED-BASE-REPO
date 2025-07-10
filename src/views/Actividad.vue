<template lang="pug">
  .curso-main-container.pb-3
    BannerInterno(icono="far fa-question-circle" titulo="Actividad didáctica")
    .container.tarjeta.tarjeta--blanca.p-4.p-md-5
  
      #Actividad
      <ActividadCompletar :parrafo="parrafo"/>
  
  </template>

<script>
import ActividadCompletar from '@/views/plantilla/actividadCompletar/ActividadCompletar.vue'
export default {
  name: 'ActividadDidactica',
  components: {
    ActividadCompletar,
  },
  data: () => ({
    parrafo: {
      tema: 'Nombre del CF',
      titulo: 'Completar palabras',
      introduccion:
        '<b> Objetivo:</b> Evaluar la comprensión y el uso adecuado de  <em>Blockchain</em> y las palabras claves asociadas.',
      instruccion:
        'Digite en el espacio indicado la palabra que dé sentido a cada frase.',
      imagen: require('@/assets/actividad/imagen1.png'),
      textos: [
        {
          id: 1,
          texto:
            'La [respuesta], como rama de la biología que estudia la estructura de los seres vivos, está relacionada con la elaboración de prendas, ya que permite lograr una mejor adaptación y funcionalidad al cuerpo humano.',
          respuesta: 'anatomía',
        },
      ],
      titulo_aprobado: '¡BUEN TRABAJO!..',
      titulo_reprobado: 'VUELVA A INTENTARLO..',
      mensaje_final_aprobado: '¡Excelente! Ha superado la actividad.',
      mensaje_final_reprobado:
        'Le recomendamos volver a revisar el componente formativo e intentar nuevamente la actividad didáctica.',
    },
  }),
}
</script>
