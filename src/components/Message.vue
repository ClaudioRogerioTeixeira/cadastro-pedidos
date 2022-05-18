<template>
  <div class="message-container">
    <p>{{msg}}</p>
  </div>
</template>

<script>
  export default {
    name: 'Message',
    props: {
      "msg": String
    }
  }
</script>

<style scoped>
  .message-container {
    color: #fff;
    background-color: rgba(10, 180, 16, 0.796);
    border: 2px solid #12811fb7;
    border-radius: 5px;
    padding: 10px;
    min-width: 400px;
    max-width: 400px;
    margin: 30px auto;
    bottom: 0;
    position: fixed;
    z-index: 1000;
    box-shadow: 5px 5px rgba(0, 0, 0, 90);
  }
</style>