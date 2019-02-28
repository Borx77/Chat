<template>
  <div class="login fuente">
    <h1 class="text-center pb-1">Login</h1>
    <div class="fondo flex justify-center pt-24">
      <div class="flex-col">
        <div class="pb-24">
          <select class="my-5 selector" v-model="emisorSeleccionado">
            <option value="0">Selecciona tu usuario</option>
            <option
              v-for="usuario in usuarios"
              :key="usuario.id"
              :value="usuario.id"
            >{{ usuario.fields.Username }}</option>
          </select>
        </div>
        <div class="text-center">
          <router-link
            v-if="emisorSeleccionado != 0"
            :to="{ name: 'list', params: {emisor: emisorSeleccionado }}"
          >
            <span class="boton-entrar">Entrar</span>
          </router-link>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Loading from "@/components/Loading.vue";

export default {
  name: "login",
  components: {},
  data: function() {
    return {
      usuarios: [],
      emisorSeleccionado: 0
    };
  },
  mounted: function() {
    //$route.params.id

    //  obtenemos los datos de airtable
    let that = this;
    // Obtenenmos el nombre del receptor
    axios
      .get(
        "https://api.airtable.com/v0/appnLtER6XsQjN5RQ/Usuarios?maxRecords=5&view=Grid%20view"
      )
      .then(function(response) {
        that.usuarios = response.data.records;
        // handle success
        console.log(response);
      })
      .catch(function(error) {
        // handle error
        console.log(error);
      })
      .then(function() {
        // always executed
      });
  }
};
</script>

<style lang="postcss" scoped>
.fondo {
  background: #c39146;
  background: -moz-linear-gradient(left, #c39146 0%, #6dcaa3 100%);
  background: -webkit-gradient(
    left top,
    right top,
    color-stop(0%, #c39146),
    color-stop(100%, #6dcaa3)
  );
  background: -webkit-linear-gradient(left, #c39146 0%, #6dcaa3 100%);
  background: -o-linear-gradient(left, #c39146 0%, #6dcaa3 100%);
  background: -ms-linear-gradient(left, #c39146 0%, #6dcaa3 100%);
  background: linear-gradient(to right, #c39146 0%, #6dcaa3 100%);
  filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#c39146', endColorstr='#6dcaa3', GradientType=1 );
  height: 96vh;
}

.selector {
  width: 14rem;
  height: 2rem;
}
.boton-entrar {
  background-color: black;
  color: white;
  padding: 0.5rem;
  padding-left: 1.2rem;
  padding-right: 1.2rem;
  border: 2px solid black;
  border-radius: 5px;
}

.fuente {
  font-family: "PT Sans Narrow", sans-serif;
}
</style>

