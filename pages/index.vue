<template>

  <div >

    <Scene/>

    <main class="mainbox">
      <Header/>
      <Sausage/>
      <Section/>
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

<style>
  @import url('https://fonts.googleapis.com/css2?family=Shadows+Into+Light&family=Signika+Negative&display=swap');

  *{
    font-size: 18px;
    font-family: 'Signika Negative', sans-serif;
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
    z-index: 99;
  }

</style>