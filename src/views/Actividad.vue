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
            'En el ámbito de la informática, [respuesta] información consiste en ocultarla, de forma que solo pueda interpretarse si se dispone de una clave o un código.',
          respuesta: 'cifrar',
        },
        {
          id: 2,
          texto:
            'Los [respuesta] son los equipos que forman parte de la red Blockchain, encargados de almacenar y distribuir en tiempo real copias actualizadas de las operaciones que se realizan.',
          respuesta: 'nodos',
        },
        {
          id: 3,
          texto:
            'Tradicionalmente, [respuesta] es el nombre con el que se llama a las piezas que, adquiridas a cambio de dinero, sirve para recibir un bien o servicio.',
          respuesta: 'token',
        },
        {
          id: 4,
          texto:
            '[respuesta] es la base de datos transaccional distribuida, formada por cadenas de bloques diseñadas para evitar su modificación una vez que un dato ha sido publicado.',
          respuesta: 'Blockchain',
        },
        {
          id: 5,
          texto:
            'Se puede gozar de total [respuesta], dependiendo de la necesidad puntual de cada aplicación.',
          respuesta: 'anonimato',
        },
        {
          id: 6,
          texto:
            'La [respuesta] es una técnica destinada a alterar las representaciones lingüísticas de ciertos mensajes con el fin de hacerlos ininteligibles a receptores no autorizados.',
          respuesta: 'criptografía',
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
