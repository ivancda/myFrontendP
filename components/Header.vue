<template>
  <div class="d-flex flex-column align-itens-center justify-content-sm-between mainBox">
    <b-container class="col12 titleBox">
      <h1>Ivan Coelho</h1>
      <h2>Frontend Dev</h2>
    </b-container>

    <b-container class="buttonBox d-flex align-itens-center justify-content-sm-around">
      <b-link>Contato</b-link>
      <b-link>Portfolio</b-link>
      <b-link>Sobre</b-link>
    </b-container>

  </div>
</template>

<script>
export default {

}
</script>

<style scoped>
  @font-face {
    font-family: hdrFont;
    src: url('../assets/fonts/GloriaHallelujah-Regular.ttf');
  }

  .mainBox{
    height: 100vh;
  }

  .titleBox{
    font-family: hdrFont;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    margin-top: 3rem;
  }

  .buttonBox{
    font-family: hdrFont;
    color: white;
    font-size: 2rem;
    margin-bottom: 6rem;
  }
  
</style>