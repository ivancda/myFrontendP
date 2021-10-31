<template>
  <div>
    <b-navbar toggleable="lg" class="col-12">
      <!-- <b-navbar-brand class="hdrName">Ivan Coelho</b-navbar-brand> -->

      <b-navbar-nav class="ml-auto">
        
        <b-container class="bBox">
          
          <b-button href="https://github.com/ivancda">
            <b-icon-github></b-icon-github>
          </b-button>
          <b-button href="https://www.linkedin.com/in/ivancda/">
            <b-icon-linkedin></b-icon-linkedin>
          </b-button>
          <b-button href="https://www.instagram.com/ivnzin/">
            <b-icon-instagram></b-icon-instagram>
          </b-button>
          <b-button href="https://www.facebook.com/ivancdaraujo/">
            <b-icon-facebook></b-icon-facebook>
          </b-button>
          
          
        </b-container>
        
      </b-navbar-nav>

    </b-navbar>
    
    <b-container class="box">
      <div class="col-6"></div>
      <div class="col-6">
        <h2>Arraste sua tela</h2>
      </div>
      
    </b-container>

  </div>
</template>

<script>
export default {

}
</script>

<style scoped>
  .box{
    margin-top: 40vh;
    display: flex;
  }

  .bBox{
    gap: 1rem;
  }

  h2{
    text-align: center;
    font-size: 8vh;
    font-family: 'Shadows Into Light', cursive;
  }
</style>