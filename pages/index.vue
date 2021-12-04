<template>

  <main >

    <Scene :pos="gotoPos"/>
    
    <div v-show="gotoPos>0" class="controlBox">
      <button @click="backward">previous</button>
      <button @click="foward">next</button>
    </div>

    <div class="mainbox">
      <Header 
      v-if="gotoPos===0"
      />
      <Portfolio 
      v-if="gotoPos>0"
      :pos="gotoPos"
      />
      <About 
      v-if="gotoPos===-2"
      />
      <Contato
      v-if="gotoPos===-1"
      />
    </div>

    <div class="buttonBox d-flex align-items-center justify-content-center">
      <button v-show="gotoPos!==0" @click="setPos(0)">Home</button>
      <button v-show="gotoPos<=0" @click="setPos(1)">Portfolio</button>
      <button v-show="gotoPos!==-2" @click="setPos(-2)">About</button>
      <button v-show="gotoPos!==-1" @click="setPos(-1)">Get in touch</button>
    </div>

  </main>

</template>

<script>
// import Section from '~/components/Section.vue'

  export default {

  css:['@/assets/css/main.scss'],
  
  // components: { Section },

  data() {
    return {
      gotoPos: 0,
    }
  },

  // mounted() {
  // },

  methods: {
    foward(){
      const newPos = this.gotoPos
      if (newPos<8) {
        this.gotoPos = newPos+1
      }
      else{
        this.gotoPos = 1
      }
    },

    backward(){
      const newPos = this.gotoPos
      if (newPos>1) {
        this.gotoPos = newPos-1
      }
      else{
        this.gotoPos = 8
      }

    },

    setPos(pos){
      this.gotoPos = pos
    }
  },

}
</script>

<style scoped>

  .controlBox{
    width: 10rem;
    position: absolute;
    bottom: 10vh;
    left: calc(50vw - 5rem);
    z-index: 1;
  }
  .controlBox button{
    color: black;
  }

  .buttonBox{
    width: 40vw;
    color: white;
    font-size: 2rem;
    padding-top: .5rem;
    gap: 2rem;
    color: #FFFFFF;
    z-index: 1;
    position: absolute;
    top: 0;
    left: calc(50vw - 20vw);
  }
  
  .buttonBox button{
    color: white;
    font-size: 1rem;
    background-color: rgba(0, 0, 0, .6);
  }

  
</style>