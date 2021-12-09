<template>
  <div class="portContainer d-flex align-items-center justify-content-center">
    
    <div class="elTitle">
      <h3>{{ title }}</h3>
    </div>

    <div class="contentBox">
      <p class="description">{{ desc }}</p>
    <div class="align-self-center">
      <p v-show="link1!==''">Check the <a :href="link1">code</a>!</p>
      <p v-show="link2!==''">Check the <a :href="link2">live page</a>!</p>
    </div>
    </div>

  </div>
</template>

<script>
export default {

  props:{
    pos: {
      type: Number,
      required: false,
      default: null
    },
  },

  data(){
    return {

      title:'',
      desc:'',
      link1:'',
      link2:'',  

      projetos: [
        {
          nome: 'IP Address Tracker',
          linkRepo: 'https://github.com/ivancda/frontendMentor7',
          linkLive: 'https://ivancda.github.io/frontendMentor7/',
          descricao: 'Ip geolocation tracker using GeoIP API and Leaflet API.',
        },
        {
          nome: 'Loopstudios Landing Page',
          linkRepo: 'https://github.com/ivancda/frontendMentor2',
          linkLive: 'https://ivancda.github.io/frontendMentor2/',
          descricao: '3 section Loopstudios landing page, CSS is on point.',
        },
        {
          nome: 'This website.',
          linkRepo: 'https://github.com/ivancda/myFrontendP',
          linkLive: '',
          descricao: 'Made with Three.js and GSAP on nuxt.js1.',
        },
        {
          nome: 'Hotel API Rest',
          linkRepo: 'https://github.com/ivancda/hotelAPIrest',
          linkLive: '',
          descricao: 'An API Rest using node.js (express).',
        },
        {
          nome: 'Todo APP',
          linkRepo: 'https://github.com/ivancda/frontendMentor6',
          linkLive: 'https://ivancda.github.io/frontendMentor6/',
          descricao: 'A todo app made with vanilla js, drag and drop, active states and 2 themes.',
        },
        {
          nome: 'NBA SQL Database',
          linkRepo: 'https://github.com/ivancda/resilia-modulo3',
          linkLive: '',
          descricao: 'Database modeling and manipulation usin Mysql Workbench',
        },
      ]
    }
  },

  watch:{
    pos(val){
      // console.log(val)
      if (val<7) {
        this.portContent(val)
      }
    },
  },

  mounted(){
    this.portContent(1)
  },

  methods:{
    portContent(position){
      const pos = position-1
      let content = {}
      if (pos>=0) {
        content = this.projetos[pos]
      }
      this.title = content.nome
      this.desc = content.descricao
      this.link1 = content.linkRepo
      this.link2 = content.linkLive
    }
  }

}
</script>

<style scoped>

  .elTitle{
    position: absolute;
    top: 6rem;
    z-index: 1;
    width: 80vw;
    display: flex;
    justify-content: center;
  }

  h3{
    background-color: rgba(0, 0, 0, .6);
    font-size: 1.4rem;
    color: white;
  }

  .contentBox{
    position: absolute;
    bottom: 5rem;
    z-index: 1;
    display: flex;
    flex-direction: column;
    align-items: baseline;
    padding: 1rem;
  }

  .contentBox p{
    background-color: rgba(0, 0, 0, .6);
    margin: 0;
    color: white;
  }

  .description{
    margin-bottom: 1rem !important;
  }

  /* .contentBox a{
    background-color: rgba(0, 0, 0, .6);
  } */

</style>