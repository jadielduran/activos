<template>
  <div class="container">
    <h2>Activos</h2>

    <div class="card">
      <div class="card-content">
        <form @submit.prevent="nuevo()">
          <p>Tipo Activo: <input type="text" v-model="payload.NameArea" required/></p>
          <p>Marca: <input type="text" v-model="payload.NameAdmin" required/></p>
          <p>Modelo: <input type="text" v-model="payload.NameAdmin" required/></p>
          <p v-if="areas.length > 0">
            <select v-model="payload.AreaId">
            <option :value="areas.id" v-for="area in areas">{{area.NameArea}}</option>
            </select>
          </p>
          <button type="submit" class="waves-effect waves-light btn-small">Agregar</button>
        </form>
      </div>
    </div>
    
    <div class="card">
      <div class="card-content">
        <table class="striped">
          <thead>
            <tr>
              <th>Tipo Activo</th>
              <th>Marca</th>
              <th>Modelo</th>
              <th>Estado</th>
              <th></th>
            </tr>
          </thead>

          <tbody>
            <tr v-for="item in items">
              <td>{{ item.Type }}</td>
              <td>{{ item.MarkName }}</td>
              <td>{{ item.Model }}</td>
              <td>{{ item.Status }}</td>
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
  name: 'ActivosView',
  data(){
    const api = process.env.VUE_APP_API;
    return{
      api,
      items:[],
      areas:[],
      payload:{
        Type:null,
        MarkName:null,
        Model:null,
        Status:"Nuevo",
        AreaId:null
      }
    }
  },
  methods: {
    update(id) {
            this.$router.push('/Activos/' + id);
        },
    eliminar(id) {
            if (confirm("Esta seguro de eliminar?.")) {
                this.axios({
                    method: 'delete',
                    url: this.api + '/Activos/' + id
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
        url: this.api+'/Activos',
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
        url: this.api+'/Activos'
      }).
      then((response)=>{
        this.items = response.data;
      }).
      catch((error)=>{
        console.log(error);
      })
    },
    getAreasList() {
      this.axios({
        method: 'get',
        url: this.api + '/Areas'
      }).
      then((response) => {
        this.areas = response.data;
        const intervalo = setTimeout(() => {
          this.intSelect(); 
          clearTimeout(intervalo);
       }, 3000);
    
      }).
      catch((error) => {
        console.log(error);
      })
    },
    intSelect(){
      this.getList();
      this.getAreasList();
      var elems = document.querySelectorAll('select');
      var instances = M.FormSelect.init(elems);
    }
  },
  components: {
  },
  mounted(){
    this.getList();
    this.getAreasList();
    var elems = document.querySelectorAll('select');
    var instances = M.FormSelect.init(elems);
  }
}
</script>
