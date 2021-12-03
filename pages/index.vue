<template>

  <div >

    <Scene :pos="gotoPos"/>
    
    <div class="devBox">
      <button @click="backward">backward </button>
      <button @click="foward">foward </button>
    </div>

    <main class="mainbox">
      <Header/>
      <Portfolio/>
      <!-- <Contato/> -->
    </main>

  </div>

</template>

<script>
// import Section from '~/components/Section.vue'

  export default {

  css:['@/assets/css/main.scss'],
  
  // components: { Section },

  data() {
    return {
      gotoPos: '0',
    }
  },

  // mounted() {
  // },

  methods: {
    foward(){
      const newPos = +this.gotoPos
      if (newPos<5) {
       this.gotoPos = `${newPos+1}`
      }
    },

    backward(){
      const newPos = +this.gotoPos
      if (newPos>0) {
       this.gotoPos = `${newPos-1}`
      }

    }
  },

}
</script>

<style scoped>
  @font-face {
    font-family: mainFont;
    src: url('../assets/fonts/Montserrat-Regular.ttf');
  }
    

  *{
    font-size: 18px;
    font-family: mainFont;
    padding:0;
    margin: 0;
    color: white;
  }

  .mainbox{
    
    width: 100%;


  }

  main{
    position: absolute;
    top: 0;
    left: 0;
    /* z-index: 99; */
  }

  .devBox{
    position: absolute;
    top: 0;
    left: 0;
    z-index: 2;
  }
  .devBox button{
    color: black;
  }

  
</style>