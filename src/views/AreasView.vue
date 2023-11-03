<template>
  <h1>Areas</h1>

  <form @submit.prevent="nuevo()">
    <p>Nombre Area: <input type="text" v-model="payload.NameArea" required/></p>
    <p>Nombre Encargado: <input type="text" v-model="payload.NameAdmin" required/></p>
    <p>Numero de Empleados: <input type="number" v-model="payload.NumEmployes" required/></p>
    <button type="submit" >Agregar</button>
  </form>

  <table class="striped">
        <thead>
          <tr>
              <th>Id</th>
              <th>Area</th>
              <th>Encargado</th>
              <th>Num. Empleados</th>
          </tr>
        </thead>

        <tbody>
          <tr v-for="item in items">
            <td>{{ item.Id }}</td>
            <td>{{ item.NameArea }}</td>
            <td>{{ item.NameAdmin }}</td>
            <td>{{ item.NumEmployes }}</td>
          </tr>
        </tbody>
      </table>
</template>

<script>

export default {
  name: 'AreasView',
  data(){
    const api = process.env.VUE_APP_API;
    return{
      api,
      items:[],
      payload:{
        NameArea:null,
        NameAdmin:null,
        NumEmployes:null
      }
    }
  },
  methods: {
    nuevo(){
      this.axios({
        method: 'post',
        url: this.api+'/Areas',
        data: this.payload
      }).
      then((response)=>{
        //this.getList();
        console.log(response);
      }).
      catch((error)=>{
        console.log(error);
      })
    },
    getList(){
      this.axios({
        method: 'get',
        url: this.api+'/Areas'
      }).
      then((response)=>{
        this.items = response.data;
      }).
      catch((error)=>{
        console.log(error);
      })
    }
  },
  components: {
  },
  mounted(){
    this.getList();
  }
}
</script>
