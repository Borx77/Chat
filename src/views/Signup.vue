<template>
  <div class="signup fuente">
    <Loading v-if="loading"></Loading>
    <router-link :to="{ name: 'login'}">Login</router-link>
    <router-link :to="{ name: 'signup'}">Signup</router-link>
    <form>
      <input
        type="text"
        v-model="username"
        @input="retardoComprobarUsername"
        class="border border-black"
      >
      <p v-if="usernameError" class="text-red">El usuario ya existe</p>
      <input
        type="password"
        @input="comprobarPassword"
        v-model="password"
        class="border border-black"
      >
      <input
        type="password"
        @input="comprobarPassword"
        v-model="passwordRepeat"
        class="border border-black"
      >
      <p v-if="passwordError" class="text-red">Las contraseñas no son iguales</p>
      <input type="submit" value="registrarse">
    </form>
  </div>
</template>

<script>
import Loading from "@/components/Loading.vue";

export default {
  name: "signup",
  components: {
    Loading
  },
  data: function() {
    return {
      loading: false,
      username: "",
      usernameError: false,
      password: "",
      passwordRepeat: "",
      passwordError: false
    };
  },
  mounted: function() {
    //$route.params.id

    //  obtenemos los datos de airtable
    let that = this;
  },
  methods: {
    comprobarUsername: function() {
      let that = this;
      axios
        .get(
          `https://api.airtable.com/v0/appnLtER6XsQjN5RQ/Usuarios?view=Grid%20view&filterByFormula=AND(UPPER({Username})="${that.username.toUpperCase()}")`
        )
        .then(function(response) {
          if (response.data.records.length > 0) {
            that.usernameError = true;
          } else {
            that.usernameError = false;
          }
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
    },
    retardoComprobarUsername: function() {
      setTimeout(this.comprobarUsername, 201);
    },
    comprobarPassword: function() {
      let that = this;
      axios
        .get(
          `https://api.airtable.com/v0/appnLtER6XsQjN5RQ/Usuarios?view=Grid%20view&filterByFormula=AND({Password}="${
            that.password
          }")`
        )
        .then(function(response) {
          if (password != passwordRepeat) {
            that.passwordError = true;
          } else {
            that.passwordError = false;
          }
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

