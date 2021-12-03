<template>
  <div class="d-flex flex-column align-items-center mainBox">
    

    <div class="buttonBox d-flex align-items-center justify-content-center">
      <b-link href="#elPort">Portfolio</b-link>
      <b-link >About</b-link>
      <b-link href="#elContat">Get in touch</b-link>
      
      
    </div>

    <div class="col12 titleBox">
      <h1>Ivan Coelho</h1>
      <h2>Fullstack Web Dev</h2>
    </div>

  </div>
</template>

<script>
export default {

}
</script>

<style scoped>
  @font-face {
    font-family: hdrFont;
    src: url('../assets/fonts/RobotoMono-Regular.ttf');
  }

  .mainBox{
    height: 100vh;
  }

  .titleBox{
    font-family: hdrFont;
    display: flex;
    flex-direction: column;
    padding: .5rem;
    border: 1px solid white;
    color: #FFFFFF;
  }

  .buttonBox{
    font-family: hdrFont;
    color: white;
    font-size: 2rem;
    padding-bottom: 4rem;
    padding-top: 2rem;
    gap: 3rem;
    color: #FFFFFF;
    width: 100%;
  }
  
  .buttonBox a{
    color: white;
    font-size: 1rem;

  }

</style>