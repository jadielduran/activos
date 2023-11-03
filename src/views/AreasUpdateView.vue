<template>
  <div class="container">
    <h2>Areas</h2>

    <div class="card">
      <div class="card-content">
        <form @submit.prevent="update()">
          <p>Nombre Area: <input type="text" v-model="payload.NameArea" required/></p>
          <p>Nombre Encargado: <input type="text" v-model="payload.NameAdmin" required/></p>
          <p>Numero de Empleados: <input type="number" v-model="payload.NumEmployes" required/></p>
          <p>
            <label>
              <input type="checkbox" class="filled-in" v-model="payload.active" />
              <span>Activo</span>
            </label>
          </p>
          <button type="submit" class="waves-effect waves-light btn-small">Editar</button>
        </form>
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
        active:null
      }
    }
  },
  methods: {
    getOne() {
            this.axios({
                method: 'get',
                url: this.api + '/areas/' + this.$route.params.id
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
                    url: this.api + '/areas/' + this.$route.params.id,
                    data: this.payload
                }).
                then((response) => {
                    console.log(response);
                }).
                catch((error) => {
                    console.log(error);
                });
            }
        }
  },
  components: {
  },
  mounted(){
    this.getOne();
  }
}
</script>
