<template>
  <div class="portContainer d-flex align-items-center justify-content-center" id="elPort">
    
    <div class="portInnerCont d-flex ">
      <div
      class="portItem d-flex flex-column "
      v-for="(item,i) in projetos"
      :key="i"
      >
      <div class="imgCont">
        <img
          class="img"
          :src="item.linkImg"
          alt="image slot"
        >
      </div>
      <div class="content">
        <h3 >{{item.nome}}</h3>
        <p >{{item.descricao}}, check <b-link :href="item.linkLive">live-page</b-link> or the <a :href="item.linkLive">repo</a>!</p>
      </div>
      </div>
      <div class="turn flex-column align-items-center justify-content-center">
        <p>Turn Your Device!</p>
        <b-icon icon="phone" class="mb-4" animation="cylon-vertical" font-scale="1.5"></b-icon>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  data(){
    return {
      projetos: [
        {
          nome: 'IP Address Tracker',
          linkImg: 'https://raw.githubusercontent.com/ivancda/frontendMentor7/main/screen.png',
          linkRepo: '',
          linkLive: '',
          descricao: 'Ip geolocation tracker, made with two APIs and a lot of vanilla javascript',
        },
        {
          nome: 'Loopstudios Landing Page',
          linkImg: 'https://raw.githubusercontent.com/ivancda/frontendMentor2/main/screen.png',
          linkRepo: 'https://github.com/ivancda/frontendMentor2',
          linkLive: 'https://ivancda.github.io/frontendMentor2/',
          descricao: '3 section landing page with',
        },
        {
          nome: 'Loopstudios Landing Page',
          linkImg: 'https://raw.githubusercontent.com/ivancda/frontendMentor2/main/screen.png',
          linkRepo: 'https://github.com/ivancda/frontendMentor2',
          linkLive: 'https://ivancda.github.io/frontendMentor2/',
          descricao: '3 section landing page with',
        },
      ]
    }
  }

}
</script>

<style scoped>

  .portContainer{
    min-height: 100vh;
    /* padding: 5vw; */
    background-color: rgba(0, 0, 0, .8);
  }

  .portInnerCont{
    overflow-x: auto;
    gap: 3rem;
    padding: 3rem;
  }

  .portItem{
    gap: 1rem;
  }


  .imgCont{
    width: 60vw;
    max-width: 50rem;
  }

  .img{
    width: 100%;
    border-radius: 50px;
  }

  .content{
    margin-left: 5vw;
    margin-right: 5vw;
  }

  .turn{
    background-color: black;
    position: absolute;
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
    z-index: 99;
    display: none;
  }

  .glowEl {
  color: black;
  text-shadow:    
      0 0 5px #fff,
      0 0 10px #fff,
      0 0 21px #fff;
  }

  

  /* @media (min-width: 1200px) {
    .portContainer{
      padding: 15vw;
    }
  } */

  /* @media only screen and (orientation:portrait){
    .turn{
      display: flex;
    }
  } */

</style>