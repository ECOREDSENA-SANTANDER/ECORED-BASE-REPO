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
      titulo_aprobado: '¡BUEN TRABAJO!..',
      titulo_reprobado: 'VUELVA A INTENTARLO..',
      mensaje_final_aprobado: '¡Excelente! Ha superado la actividad.',
      mensaje_final_reprobado:
        'Le recomendamos volver a revisar el componente formativo e intentar nuevamente la actividad didáctica.',
    },
  }),
}
</script>
