<template>
  <Banner />
  <div class="main-container">
    <h2>Selecione os itens do formulário:</h2>
    <BurgerForm />
  </div>
</template>

<script>
 import Banner from '../components/Banner.vue';
 import BurgerForm from '../components/BurgerForm.vue';

 export default {
  name: 'HomeView',
  components: {
    Banner,
    BurgerForm
  }
 }

</script>

<style>
  h2 {
    display: flex;
    justify-content: center;
    margin-bottom: 15px;
  }
</style>
