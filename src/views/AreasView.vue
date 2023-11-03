<template>
  <div class="container">
    <h2>Areas</h2>

    <div class="card">
      <div class="card-content">
        <form @submit.prevent="nuevo()">
          <p>Nombre Area: <input type="text" v-model="payload.NameArea" required/></p>
          <p>Nombre Encargado: <input type="text" v-model="payload.NameAdmin" required/></p>
          <p>Numero de Empleados: <input type="number" v-model="payload.NumEmployes" required/></p>
          <button type="submit" class="waves-effect waves-light btn-small">Agregar</button>
        </form>
      </div>
    </div>
    
    <div class="card">
      <div class="card-content">
        <table class="striped">
          <thead>
            <tr>
              <th>Id</th>
              <th>Area</th>
              <th>Encargado</th>
              <th>Num. Empleados</th>
              <th></th>
            </tr>
          </thead>

          <tbody>
            <tr v-for="item in items">
              <td>{{ item.id }}</td>
              <td>{{ item.NameArea }}</td>
              <td>{{ item.NameAdmin }}</td>
              <td>{{ item.NumEmployes }}</td>
              <td>
                <a class="app-btn btn-small btn-floating btn-large waves-effect waves-light red"><i class="material-icons" @click="eliminar(item.id)" >delete</i></a>
                <a class="app-btn btn-small btn-floating btn-large waves-effect waves-light blue "><i class="material-icons" @click="update(item.id)" >edit</i></a>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>    
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
        NumEmployes:null,
        active:true
      }
    }
  },
  methods: {
    update(id) {
            this.$router.push('/Areas/' + id);
        },
    eliminar(id) {
            if (confirm("Esta seguro de eliminar?.")) {
                this.axios({
                    method: 'delete',
                    url: this.api + '/Areas/' + id
                }).
                then((response) => {
                    this.getList();
                }).
                catch((error) => {
                    console.log(error);
                });
            }
        },
    nuevo(){
      this.axios({
        method: 'post',
        url: this.api+'/Areas',
        data: this.payload
      }).
      then((response)=>{
        this.getList();
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
