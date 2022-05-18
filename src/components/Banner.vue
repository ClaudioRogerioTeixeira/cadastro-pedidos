<template>
<div id="main-banner">
  <h1>Monte seu Hambúrguer</h1>
  <div id="banner-text"> 
    <p>O lanche mais quente do Brasil.</p>
    <p>Escolha os ingredientes e saborei seu lanche.</p>
    <p>Utilizamos produtos frescos de ótima qualidade.</p>
  </div>
</div>
</template>

<script>
  export default {
    name: 'Banner'
  }

</script>

<style scoped>
  #main-banner {
    width: auto;
    min-height: 460px;
    color: #fff;

    display: flex;
    flex-direction: column;
    justify-content: center;

    background-image: url('/public/img/burgerFire.jpg');
    background-size: cover;    
  }

  #banner-text {
    display: flex;
    flex-direction: column;
    gap: 7px;
  }

  h1 {
    margin-left: 60px;
    margin-bottom: 15px;
    font-size: 50px;
  }

  p {
    margin-left: 60px;
  }

  @media only screen and (max-width: 600px) {
    #main-banner {
      display: flex;
      justify-content: start;
      padding-top: 30px;
      background-image: url('/public/img/burgerSmall.jpg');
    }
  }  

</style>