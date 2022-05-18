<template>
  <nav>
    <router-link to="/" id="logo_url"><img v-bind:src="logo" v-bind:alt="alt" id="logo"></router-link>
    <router-link to="/">Home</router-link>
    <router-link to="/pedidos">Pedidos</router-link>
  </nav>
</template>

<script>
export default {
  name: 'Navbar',
  props:["logo", "alt"]
}

</script>

<style scoped>
  nav {
    color: #fff;
    background-color: #222;
    width: 100%;
    min-height: 50px;
    padding: 20px 50px;
    display: flex;
    justify-content: flex-end;
    align-items: center;
    gap: 20px;
  }

  nav a, :hover {
    color: #fff;
    text-decoration: none;
    transition: .5s;
  }

  nav a:hover {
    color: rgb(249, 112, 0);
  }

  #logo_url {
    margin: auto;
    margin-left: 10px;
  }

  #logo {
    width: 200px

  }

  @media only screen and (max-width: 600px) {
    #logo {
      width: 100px;
    }
  }
</style>