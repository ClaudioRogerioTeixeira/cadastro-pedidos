<template>
  <div id="burger-table">
    <div>
      <div id="burger-table-heading">
        <div class="order-id">Id</div>
        <div>Cliente</div>
        <div>Celular</div>
        <div>Pão</div>
        <div>Carne</div>
        <div>Opcionais</div>
        <div>Status</div>
        <!-- <div><md-icon>add</md-icon></div> -->
        <div>Ação</div>
      </div>
    </div>
    <div v-show="!burgers" class="sem-registro"><span>Nenhum registro cadastrado.</span></div>
    <div id="burger-table-rows" v-show="burgers">
      <div class="burger-table-row" v-for="row in burgers" :key="row.id">
        <div class="order-number">{{row.id}}</div>
        <div><span></span>{{row.nome}}</div>
        <div>{{row.celular}}</div>
        <div>{{row.pao}}</div>
        <div>{{row.carne}}</div>
        <div>
          <ul>
            <li v-for="(opcional, index) in row.opcionais" v-bind:key="index">{{opcional}}</li>           
          </ul>
        </div>
        <div>
          <select name="status" id="status" @change="updateBurger($event, row.id)">
            <option value="">Selecione:</option>
            <option v-for="item in status" v-bind:key="item.id" :value="item.tipo" :selected="row.status == item.tipo">{{item.tipo}}</option>
          </select>
        </div>
        <div>
          <button class="delete-btn" v-on:click="deletePedido(row.id)">Cancelar</button>
        </div>
      </div>   
    </div>
  </div>
  <div id="position-msg"><Message v-bind:msg="msg" v-show="msg" /></div>
</template>

<script>
  import MenuIcon from 'vue-material-design-icons/Menu.vue';
    import Message from './Message.vue';

  export default {
    name: 'Dashboard',
    components: {
      MenuIcon,
      Message
    },
    data() {
      return {
        status: null,
        burgers: null,
        status: [],
        msg: null,
        baseUrl: 'https://apppedidosapi.herokuapp.com',
      }
    },

    methods: {

      // Carrega os Pedidos
      async getPedidos() {
        const req = await fetch(`${this.baseUrl}/burgers`);
        this.burgers = await req.json();
        console.log('this.burgers', this.burgers)
      },

      // Carrega os Status
      async getStatus() {
        const req = await fetch(`${this.baseUrl}/status`);
        this.status = await req.json();
      },

      // Deleta Pedidos
      async deletePedido(id) {
        const req = await fetch(`${this.baseUrl}/burgers/${id}`, {
          method: 'DELETE'
        })

        const res = await req.json()

        this.msg = `Registro ${id} excluido com sucesso`;

        // limpar mensagem
        setTimeout(() => {
          this.msg = '';
        }, 2000);

        this.getPedidos();
      },

      async updateBurger(event, id) {
        const option = event.target.value;
        const dataJson = JSON.stringify({ status: option });

        const req = await fetch(`${this.baseUrl}/burgers/${id}`, {
          method: 'PATCH',
          headers: { 'Content-Type': 'application/json'},
          body: dataJson
        })

        const res = await req.json();

        // Exibe Mensagem
        this.msg = `Pedido ${res.id} foi atualizado para ${res.status}`;

        // Limpar mensagem
        setTimeout(() => {
          this.msg = '';
        }, 2000);

      }

    },
    
    mounted() {
      this.getStatus();
      this.getPedidos();
    }

  }
</script>

<style scoped>

  .burger-table {
    max-width: 1200px;
    margin: 0 auto;
  }

  #burger-table-heading,
  #burger-table-rows,
  .burger-table-row {
    display: flex;
    flex-wrap: wrap;
  }

  #burger-table-heading {
    font-weight: bold;
    padding: 12px;
    /* border: 1px solid #333; */
    background-color: rgb(234, 150, 66);
    color: white;
  }

  #burger-table-heading div,
  .burger-table-row div {
    width: 13%;
  }

  .burger-table-row {
    width: 100%;
    padding: 12px;
    border: 1px solid #CCC
  }

  .burger-table-row:hover {
    background-color: rgba(234, 150, 66, 0.2);
  }

  #burger-table-heading .order-id,
  .burger-table-row .order-number {
    width: 5%;
  }

  select {
    width: 120px;
    height: 30px;
    font-size: 12px;
    color: #000;
    background-color: rgb(247, 200, 149);
    padding: 5px;
    border: none;
    border-radius: 3px;
    cursor: pointer;
    transition: .5s;
  }

  button {
    width: 70px;
    height: 30px;
    color: #FFF;
    font-weight: bold;
    background-color: rgb(233, 132, 24);
    border: none;
    border-radius: 7px;
    cursor: pointer;
    transition: .5s;
  }

  button:hover {
    color: rgb(233, 132, 24);
    background-color: transparent;
    border: 1px solid rgb(233, 132, 24);
  }

  #position-msg {
    display: flex;
    justify-content: center;
  }

  .sem-registro {
    height: 40px;
    padding: 8px;
    color: #FFF;
    margin-top: 20px;
    background-color: rgba(210, 19, 19, 0.9);
    border-radius: 5px;
  }

</style>