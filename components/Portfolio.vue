<template>
  <div class="portContainer d-flex align-items-center justify-content-center">
    
    <div class="elTitle">
      <h3>{{ title }}</h3>
    </div>

    <div class="contentBox">
      <p>{{ desc }}</p>
        <div class="d-flex flex-column">
          <a href="">repo</a>
          <a href="">live page</a>
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

      title:'1',
      desc:'2',
      link1:'3',
      link2:'4',  

      projetos: [
        {
          nome: 'IP Address Tracker',
          linkImg: 'https://raw.githubusercontent.com/ivancda/frontendMentor7/main/screen.png',
          linkRepo: '',
          linkLive: '',
          descricao: 'Ip geolocation tracker',
        },
        {
          nome: 'Loopstudios Landing Page',
          linkImg: 'https://raw.githubusercontent.com/ivancda/frontendMentor2/main/screen.png',
          linkRepo: 'https://github.com/ivancda/frontendMentor2',
          linkLive: 'https://ivancda.github.io/frontendMentor2/',
          descricao: '3 section landing page',
        },
        {
          nome: '3',
          linkImg: 'https://raw.githubusercontent.com/ivancda/frontendMentor2/main/screen.png',
          linkRepo: 'https://github.com/ivancda/frontendMentor2',
          linkLive: 'https://ivancda.github.io/frontendMentor2/',
          descricao: '3',
        },
      ]
    }
  },

  watch:{
    pos(val){
      // console.log(val)
      if (val<4) {
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
    bottom: 8rem;
    z-index: 1;
    display: flex;
    flex-direction: column;
    align-items: baseline;
    gap: 1rem;
  }

  .contentBox p{
    background-color: rgba(0, 0, 0, .6);
    margin: 0;
    color: white;
  }

  .contentBox a{
    background-color: rgba(0, 0, 0, .6);
  }

</style>