<template>
  <div class="d-flex flex-column align-items-center mainBox">

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

  .mainBox{
    width: 5vw;
    position: absolute;
    top: 6rem;
    left: calc(50vw - 2.5vw);
    z-index: 1;
  }

  .titleBox{
    display: flex;
    flex-direction: column;
    padding: 1rem 1.9rem 1rem 1.9rem;
    border: .3rem solid white;
    color: #FFFFFF;
    background-color: rgba(0, 0, 0, .6);
  }

  .titleBox h1{
    font-size: 2rem;
  }

  .titleBox h2{
    font-size: 1.5rem;
  }

</style>