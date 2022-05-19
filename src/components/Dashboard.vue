<template>
  <div class="container">
    <div class="table-wrapper">
      <div v-show="!burgers" class="sem-registro"><span>Nenhum registro cadastrado.</span></div>
      <table v-show="burgers">
        <thead>
          <tr>
            <th>Id</th>
            <th>Cliente</th>
            <th>Celular</th>
            <th>Pão</th>
            <th>Carne</th>
            <th>Opcionais</th>
            <th>Status</th>
            <th>Ação</th>
          </tr>
        </thead>    
        <tbody>         
          <tr v-for="row in burgers" :key="row.id">
            <td>{{row.id}}</td>
            <td>{{row.nome}}</td>
            <td>{{row.celular}}</td>
            <td>{{row.pao}}</td>
            <td>{{row.carne}}</td>
            <td>
              <ul>
                <li v-for="(opcional, index) in row.opcionais" v-bind:key="index">{{opcional}}</li>                
              </ul>
            </td>
            <td>
              <select name="status" id="status" @change="updateBurger($event, row.id)">
                <option value="">Selecione:</option>
                <option v-for="item in status" v-bind:key="item.id" :value="item.tipo" :selected="row.status == item.tipo">{{item.tipo}}</option>
              </select>
            </td>
            <td>
              <button class="delete-btn" v-on:click="deletePedido(row.id)">Cancelar</button>
            </td>
          </tr>                                               
        </tbody>
      </table>
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

  .table-wrapper {
    max-height: 600px;
    overflow-y: auto;
  }

  table {
     border-collapse: none;
     border-spacing: 0px;
     width: 100%;
  }

  th {
    position: sticky;
    top: 0;
    background-color: rgb(234, 150, 66);
    color: white;
  }

  th, td {
    padding: 10px;
  }

  tr:hover {
    background-color: rgba(234, 150, 66, 0.2);
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
    height: auto;
    width: 100%;
    padding: 8px;
    color: #FFF;
    margin-top: 20px;
    background-color: rgba(210, 19, 19, 0.7);    
  }

</style>