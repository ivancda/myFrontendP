<template>
  <div class="d-flex align-items-end justify-content-center contato">


    <div class="d-flex flex-column align-items-center justify-content-center contBox">
        
        <div class="card">
          <!-- <div class="d-flex align-items-center">
            <h4>ENDEREÇO</h4>
            <b-icon icon="map" animation="cylon-vertical" font-scale="1.5"></b-icon>
          </div>
          <p class="textCard">225, Rua Grajaú<br>Rio de Janeiro, RJ</p>
          <hr> -->
          <div class="d-flex align-item-center">
            <h4>EMAIL</h4>
            <b-icon icon="mailbox" animation="cylon-vertical" font-scale="1.5"></b-icon>
          </div>
          <p class="textCard">ivancda@gmail.com<br></p>
          <hr>
          <div class="d-flex align-item-center">
            <h4>PHONE</h4>
            <b-icon icon="phone" animation="cylon-vertical" font-scale="1.5"></b-icon>
          </div>
          <p class="textCard">+55 (21) 97528-5936</p>
        </div>
        
      <div class="linkBox d-flex align-items-center justify-content-center">
        <a href="http://"><b-icon icon="facebook"></b-icon></a>
        <a href="http://"><b-icon icon="instagram"></b-icon></a>
        <a href="http://"><b-icon icon="linkedin"></b-icon></a>
        <a href="http://"><b-icon icon="github"></b-icon></a>
      </div>
    </div>
  </div>
</template>

<script>
export default {

}
</script>

<style scoped>

  .contBox{
    position: absolute;
    bottom: 2rem;
    z-index: 1;
  }

  .card{
    display: flex;
    flex-direction: column;
    align-items: center;
    background-color: white;
    padding: 1rem;
    width: 14rem;
    border-radius: 2rem 2rem 0.5rem 0.5rem;
    border-bottom: .5rem solid black;
    text-align: center;
  }

  .card hr{
    width: 3rem;
    border-color: black;
    border-width: .25rem;
  }

  .card h4{
    letter-spacing: .5rem;
    font-weight: 900;
    color: black;
    margin: 0;
  }


  .textCard{
    height: 100%;
    display: flex;
    flex-direction: column;
    align-self: center;
    justify-content: center;
    color: black;
    margin: 0;
  }

  .linkBox{
    gap: 1rem;
    margin-top: 1rem;
  }

  .linkBox a{
    display: flex;
    align-items: center;
    justify-content: center;
    height: 2rem;
    background-color: white;
    color: black;
    padding: .5rem;
    border-radius: 50%;
    transition: 500ms;
    box-shadow: 0px -2px 0px 1px gray inset;

  }

  .linkBox a:hover{
    background-color: gray;
    color: black;
    box-shadow: 0px 0px 0px 0px gray inset;
  }

</style>