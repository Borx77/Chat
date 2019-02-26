<template>
  <div class="login">
    <h1 class="text-center">Login</h1>
    <select class="my-5" v-model="emisorSeleccionado">
      <option value="0">Selecciona tu usuario</option>
      <option
        v-for="usuario in usuarios"
        :key="usuario.id"
        :value="usuario.id"
      >{{ usuario.fields.Username }}</option>
    </select>
    <router-link
      v-if="emisorSeleccionado != 0"
      :to="{ name: 'list', params: {emisor: emisorSeleccionado }}"
    >Entrar</router-link>
  </div>
</template>

<script>
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
</style>

