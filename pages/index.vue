<template>

  <div >

    <Scene/>

    <main class="mainbox">
      <Header/>
      
    </main>

  </div>

</template>

<script>
// import Section from '~/components/Section.vue'

  export default {
  // components: { Section },

  // data() {
  //   return {
  //   }
  // },

  // mounted() {
  // },

  // methods: {
  // },

}
</script>

<style scoped>
  @font-face {
    font-family: mainFont;
    src: url('../assets/fonts/SourceSansPro-Regular.ttf');
  }
    

  *{
    font-size: 18px;
    font-family: mainFont;
    padding:0;
    margin: 0;
    color: white;
  }

  .mainbox{

    width: 100vw;

  }

  main{
    position: absolute;
    top: 0;
    left: 0;
    /* z-index: 99; */
  }

</style>