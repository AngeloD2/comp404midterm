<template>
    <v-carousel hide-delimiters >
      <v-carousel-item
        v-for="(item,i) in items"
        :key="i"
        :src="item.src"
        cover
      ></v-carousel-item>
    </v-carousel>

    
  </template>


  <script>
  import one from '../assets/1.jpg'
  import two from '../assets/2.webp'
  import three from '../assets/3.jpg'

  export default {
    data () {
      return {
        items: [
          {
            src: one,
          },
          {
            src: two,
          },
          {
            src: three
          },
        ],
      }
    },
  }
</script>