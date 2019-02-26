<template>
  <div class="list">
    <div class="w-full bg-grey-dark py-6">
      <h1 class="text-center text-white">Chat de Amigos</h1>
    </div>
    <div class="bg-white-light w-full flex justify-center">
      <div class="w-5/6 bg-grey-light m-3 p-4 text-center rounded-lg">
        <i class="fas fa-search"></i>
        <input type="text" placeholder="Buscar" class="w-2/3 h-6 ml-4 rounded">
      </div>
    </div>
    <div class="fondo flex flex-col justify-center" v-for="usuario in usuarios" :key="usuario.id">
      <router-link
        :to="{name: 'chat', params: {emisor: $route.params.emisor, receptor: usuario.id }}"
      >
        <div class="bg-white m-4 mx-8 p-2 w-5/6 flex flex-row rounded">
          <div class="w-1/6 flex justify-center">
            <img
              class="block h-16 sm:h-24 rounded-full mx-auto mb-4 sm:mb-0 sm:mr-4 sm:ml-0"
              src="https://avatars2.githubusercontent.com/u/4323180?s=400&u=4962a4441fae9fba5f0f86456c6c506a21ffca4f&v=4"
              alt
            >
          </div>
          <div class="w-3/4 mt-4 ml-3">
            <h3>{{usuario.fields.Username}}</h3>
            <p
              class="mt-3 text-xs"
            >Lorem ipsum dolor sit amet consectetur adipisicing elit. Saepe, porro!</p>
          </div>
          <div class="flex justify-center items-center">
            <i class="fas fa-chevron-right tamanyo"></i>
          </div>
        </div>
      </router-link>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";

export default {
  name: "list",
  components: {},
  data: function() {
    return {
      usuarios: [],
      loading: true
    };
  },
  mounted: function() {
    let that = this;
    axios
      .get(
        "https://api.airtable.com/v0/appnLtER6XsQjN5RQ/Usuarios?view=Grid%20view"
      )
      .then(function(response) {
        // handle success
        that.usuarios = response.data.records;
        console.log(that.usuarios);
        that.loading = false;
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
}

.tamanyo {
  font-size: 2.5rem;
}
</style>

