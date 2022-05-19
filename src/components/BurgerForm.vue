<template>
  <div id="container">  
    <form id="burger-form" @submit="createBurger">
    <Message v-bind:msg="msg" v-show="msg" />
      <div class="input-container">
        <label for="nome">Nome:</label>
        <input type="text" id="nome" name="name" v-model="nome" placeholder="Digite seu nome">
      </div>
      <div class="input-container">
        <label for="celular">Celular:</label>
        <input type="text" id="celular" name="celular" v-model="celular" placeholder="Digite seu nome">
      </div>      
      <div class="input-container">
        <label for="pao">Escolha o pão:</label>
        <select name="pao" id="pao" v-model="pao">
          <option value="">Selecione o seu pão</option>
          <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">{{pao.tipo}}</option>
        </select>
      </div>
      <div class="input-container">
        <label for="carne">Escolha a carne:</label>
        <select name="carne" id="carne" v-model="carne">
          <option value="">Selecione o tipo de carne</option>
          <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo">{{carne.tipo}}</option>
        </select>
      </div>
      <div id="opcionais-container" class="input-container">
        <label id="opcionais-title" for="opcionais">Selecione os opcionais:</label>
        <div class="checkbox-container" v-for="opcional in opcionaisdata" :key="opcional.id">
          <input type="checkbox" name="opcionais" v-model="opcionais" :value="opcional.tipo" >
          <span>{{opcional.tipo}}</span>
        </div>       
      </div>    
      <div class="input-container">
        <input type="submit" class="submit-btn" value="Criar meu Burger!"> 
      </div>
    </form>
  </div>
</template>

<script>
  import Message from './Message.vue';

  export default {
    name: 'BurgerForm',
    components: {
      Message
    },
    data() {
      return {
        paes: null,
        carnes: null,
        opcionaisdata: null,

        nome: null,
        celular: null,
        carne: null,
        pao: null,
        opcionais: [],

        msg: null,
        baseUrl: 'https://apppedidosapi.herokuapp.com'
      }
    },
    methods: {      
      
      // Carrega as carnes
      async getCarnes() {
        const req = await fetch(`${this.baseUrl}/carnes`);
        this.carnes = await req.json();
        console.log('carnes', this.carnes);
      },

      // Carrega os pães
      async getPaes() {
        const req = await fetch(`${this.baseUrl}/paes`);
        this.paes = await req.json();
        console.log('paes', this.paes);
      },

      // Carrega os opcionais
      async getOpcionais() {
        const req = await fetch(`${this.baseUrl}/opcionais`);
        this.opcionaisdata= await req.json();
        console.log('opcionaisdata', this.opcionaisdata);
      },

      // Inclui Hamburguer
      async createBurger(e) {
        e.preventDefault();

        const data = {
          nome: this.nome,
          celular: this.celular,
          carne: this.carne,
          pao: this.pao,
          opcionais: Array.from(this.opcionais),
          status: 'Solicitado'
        }

        const dataJson = JSON.stringify(data);

        const req = await fetch(`${this.baseUrl}/burgers`, {
          method: "POST",
          headers: {"Content-Type": "application/json"},
          body: dataJson
        });

        const res = await req.json();
        
        // Inclui msg de sistema
        this.msg = `Pedido nº ${res.id} realizado com sucesso`;

        // Limpar mensagem
        setTimeout(() => {
          this.msg = '';
        }, 2000);

        // Limpar campos
        this.nome = '',
        this.celular = '',
        this.carne = '',
        this.pao = '',
        this.opcionais = ''
        
      }
    },
    mounted() {
      this.getCarnes();
      this.getPaes();
      this.getOpcionais();
      // this.msg = 'Customizando Mensagem'
    }
  }
</script>

<style scoped>
  #container {
    margin-bottom: 100px;
  }
  #burger-form {
    display: flex;
    flex-direction: column;
    gap: 10px;
    max-width: 400px;
    margin: 0 auto;
  }
  .input-container {
    display: flex;
    flex-direction: column;
  }
  label {
    font-weight: bold;
    /* color: #222; */
    color: rgb(146, 89, 42);
  }
  input, select {
    padding: 5px 10px;
  }
  #opcionais-container {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    gap: 20px;
    justify-content: space-around;
  }
  #opcionais-title {
    width: 100%;
  }
  .checkbox-container span,
  .checkbox-container input {
    width: auto;
  }
  .submit-btn {
    color: #fff;
    background-color: rgb(249, 112, 0);
    border: none;
    border-radius: 6px;
    height: 40px;
    font-weight: bold;
    cursor: pointer;
    transition: .5s;
  }
  .submit-btn:hover {
  background-color: transparent;
  color: rgb(197, 90, 2);
  border: 2px solid rgb(197, 90, 2);
  }
  .checkbox-container span {
    margin-left: 6px;
    font-weight: bold;    
  }
</style>