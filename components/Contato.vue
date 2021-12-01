<template>
  <div class="d-flex align-items-end justify-content-center contato p-3">


    <div class="d-flex flex-column align-items-center justify-content-center contBox">
      <!-- <h3 class="tit">Contato:</h3> -->
      <div class="cardsBox d-flex align-items-center justify-content-center">
        <div class="card col-md-4 mb-md-0">
          <b-icon icon="map" class="mb-4" animation="cylon-vertical" font-scale="1.5"></b-icon>
          <h4>ENDEREÇO</h4>
          <hr class="my-5">
          <div class="textCard">
            <p>225, Rua Grajaú<br>Rio de Janeiro, RJ</p>
          </div>
        </div>
        <div class="card col-md-4 mb-md-0">
          <b-icon icon="mailbox" class="mb-4" animation="cylon-vertical" font-scale="1.5"></b-icon>
          <h4>EMAIL</h4>
          <hr class="my-5">
          <div class="textCard">
            <p>ivancda@gmail.com<br></p>
          </div>
        </div>
        <div class="card col-md-4 mb-md-0">
          <b-icon icon="phone" class="mb-4" animation="cylon-vertical" font-scale="1.5"></b-icon>
          <h4>TELEFONE</h4>
          <hr class="my-5">
          <div class="textCard">
            <p>+55 (21) 97528-5936</p>
          </div>
        </div>
      </div>
      <div class="linkBox d-flex align-items-center justify-content-center p-3">
        <a href="http://"><b-icon icon="facebook" font-scale="2"></b-icon></a>
        <a href="http://"><b-icon icon="instagram" font-scale="2"></b-icon></a>
        <a href="http://"><b-icon icon="linkedin" font-scale="2"></b-icon></a>
        <a href="http://"><b-icon icon="github" font-scale="2"></b-icon></a>
      </div>
    </div>
  </div>
</template>

<script>
export default {

}
</script>

<style>

  .tit{
    margin-bottom: 3rem;
  }

  .contato{
    height: 100vh;
  }

  .contBox{
    padding: 3rem;
  }

  .cardsBox{
    color: black;
    gap: 5rem;
  }

  .card{
    display: flex;
    flex-direction: column;
    align-items: center;
    background-color: white;
    padding: 2rem;
    border-radius: 2rem 2rem 0.5rem 0.5rem;
    border-bottom: .5rem solid black;
    text-align: center;
    height: 20rem;
  }

  .card hr{
    width: 3rem;
    border-color: black;
    border-width: .25rem;
  }

  .card h4{
    letter-spacing: .5rem;
    font-weight: 900;
  }

  .textCard{
    height: 100%;
    display: flex;
    flex-direction: column;
    align-self: center;
    justify-content: center;
  }

  .linkBox{
    gap: 3rem;
    margin-top: 4rem;
  }

  .linkBox a{
    background-color: black;
    color: white;
    padding: 1rem;
    border-radius: 50%;
    box-shadow: 0px -2px 0px 1px gray inset;
    transition: 500ms;
  }

  .linkBox a:hover{
    background-color: gray;
    color: black;
    box-shadow: 0;
  }

</style>