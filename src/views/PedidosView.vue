<template>
  <div class="container">
    <h2>Gerenciar pedidos</h2>
    <Dashboard />
  </div>
</template>

<script>
 import Dashboard from '../components/Dashboard.vue';

  export default {
    name: 'Pedidos',
    components: {
      Dashboard
    }
  }
</script>

<style scoped>

  .container {
    margin-top: 40px;
    padding: 0px 20px;
  }

  h2 {
    display: flex;
    justify-content: center;
    margin-bottom: 15px;
  }
</style>
