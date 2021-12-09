<template>
  <div class="d-flex align-items-end justify-content-center">
    
    <div class="d-flex flex-column align-items-center aboutContent">
      <p>Hi! I'm Ivan.</p>
      <p>
        Passionate about technology and art, I want to provide a web experience that is more than just a classic website, something that conveys true aesthetic beauty and can connect with the user discovery process.
      </p>
      <p> 
        This app was made as an exercise to train the use of advanced libraries along with three.js, and I took the opportunity to show some of my creations. I'll be updating and bringing new features in the future. Thank you.
      </p>
    </div>

  </div>
</template>

<script>
export default {

}
</script>

<style>

  .aboutContent{
    position: absolute;
    bottom: 0rem;
    z-index: 1;
    padding: 1rem;
    width: 100%;
  }

  .aboutContent p{
    background-color: rgba(0, 0, 0, .6);
    color: white;
    max-width: 30rem;
    font-size: .8rem;
  }

</style>