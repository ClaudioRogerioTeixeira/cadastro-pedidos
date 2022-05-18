<template>
  <Navbar v-bind:logo="logo_src" v-bind:alt="app_name"/>
  <router-view/>
  <Footer />
</template>

<script>
  import Navbar from './components/Navbar.vue';
  import Footer from './components/Footer.vue'

  export default {
    data() {
      return {
        logo_src: '/img/logo.png',
        app_name: 'Faça seu Hamburger'
      }
    },
    components: {
      Navbar,
      Footer
    }
  }
</script>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    text-decoration: none;
  }

  body {
    background-color: #f3f3f3;
  }

  #app {
    font-family: Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }

  .main-container {
    margin: 50px;
  }

  h1 {
    /* text-align: center; */
    font-size: 35px;
    /* color: #222; */
  }
</style>
