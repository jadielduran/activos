<template>
  <div class="container">
    <h2>Activos</h2>

    <div class="card">
      <div class="card-content">
        <form @submit.prevent="update()">
          <p>Tipo Activo: <input type="text" v-model="payload.Type" required/></p>
          <p>Marca: <input type="text" v-model="payload.MarkName" required/></p>
          <p>Modelo: <input type="text" v-model="payload.Model" required/></p>
          <p>Area: 
            <select v-model="payload.AreaId">
            <option :value="areas.id" v-for="area in areas">{{area.NameArea}}</option>
            </select>
          </p>
          <select  v-model="payload.Status">
            <option value="nuevo" selected>Nuevo</option>
            <option value="usado">Usado</option>
            <option value="desuso">Desuso</option>
          </select>
          <button type="submit" class="waves-effect waves-light btn-small">Agregar</button>
        </form>
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
        Status:null,
        AreaId:null
      }
    }
  },
  methods: {
    getOne() {
      this.axios({
        method: 'get',
        url: this.api + '/activos/' + this.$route.params.id
      }).
      then((response) => {
        this.payload = response.data;
      }).
      catch((error) => {
        console.log(error);
      });
    },
    update() {
      if (confirm("Esta seguro de editar?.")) {
        this.axios({
          method: 'patch',
          url: this.api + '/activos/' + this.$route.params.id,
          data: this.payload
        }).
        then((response) => {
          console.log(response);
        }).
        catch((error) => {
          console.log(error);
        });
      }
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
       }, 4000);
    
      }).
      catch((error) => {
        console.log(error);
      })
    },
    intSelect(){
      //this.getList();
      this.getAreasList();
      var elems = document.querySelectorAll('select');
      var instances = M.FormSelect.init(elems);
    }
  },
  components: {
  },
  mounted(){
    this.getOne();
    this.getAreasList();
    var elems = document.querySelectorAll('select');
    var instances = M.FormSelect.init(elems);
  }
}
</script>
