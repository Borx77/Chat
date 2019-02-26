<template>
  <div class="chat">
    <h2 class="text-center">Chat</h2>
    <div class="bg-green-dark w-full border border-white">
      <h2 class="text-center">{{ receptorUsername }}</h2>
    </div>
    <div class="bg-grey w-full mt-3">
      <p v-for="mensaje in mensajes" :key="mensaje.id">
        <span
          :class="{'text-green': mensaje.fields.Receptor == $route.params.receptor, 'text-red': mensaje.fields.Receptor != $route.params.receptor}"
        >{{ mensaje.fields.Texto }}</span>
      </p>
    </div>
    <div class="mt-3 fixed fixed pin-b pin-l p-5">
      <input type="text" class="border border-black" v-model="textoNuevoMensaje">
      <button @click.prevent="nuevoMensajeapi" class="p-2 bg-green ml-2">Enviar</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "chat",
  props: ["emisor", "receptor"],
  components: {},
  data: function() {
    return {
      loading: true,
      emisorUsername: "",
      receptorUsername: "",
      mensajes: [],
      textoNuevoMensaje: "",
      tiempoRefresco: 1000
    };
  },
  mounted: function() {
    //  obtenemos los datos de airtable
    let that = this;
    //OBtenemos el username del emisor
    axios
      .get(
        `https://api.airtable.com/v0/appnLtER6XsQjN5RQ/Usuarios/${
          that.$route.params.emisor
        }`
      )
      .then(function(response) {
        // Guardamos el username del receptor
        that.emisorUsername = response.data.fields.Username;

        // Obtenenmos el nombre del receptor
        axios
          .get(
            `https://api.airtable.com/v0/appnLtER6XsQjN5RQ/Usuarios/${
              that.$route.params.receptor
            }`
          )
          .then(function(response) {
            // Guardamos el username del receptor
            that.receptorUsername = response.data.fields.Username;

            // Con el resultado obtenemos los mensajes del receptor y del emisor
            that.cargarMensajes();
            // Refrescamos los mensajes
            setInterval(that.cargarMensajes, that.tiempoRefresco);

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
      })
      .catch(function(error) {
        // handle error
        console.log(error);
      })
      .then(function() {
        // always executed
      });
  },
  methods: {
    cargarMensajes: function() {
      let that = this;
      // Con el resultado obtenemos los mensajes del receptor y del emisor
      axios
        .get(
          `https://api.airtable.com/v0/appnLtER6XsQjN5RQ/Mensajes?view=Grid%20view&filterByFormula=OR(AND({Emisor}="${
            that.emisorUsername
          }", {Receptor}="${that.receptorUsername}"), AND({Emisor}="${
            that.receptorUsername
          }", {Receptor}="${that.emisorUsername}"))`
        )
        .then(function(response) {
          // handle success
          that.mensajes = response.data.records;

          that.loading = false;
        })
        .catch(function(error) {
          // handle error
          console.log(error);
        })
        .then(function() {
          // always executed
        });
    },
    nuevoMensajeapi: function() {
      let that = this;
      // Enviamos a la API el nuevo mensaje
      axios.defaults.headers.common["Authorization"] =
        "Bearer keyz8WHYGEevQhA4L";

      axios
        .post(`https://api.airtable.com/v0/appnLtER6XsQjN5RQ/Mensajes`, {
          fields: {
            Texto: that.textoNuevoMensaje,
            Emisor: [that.$route.params.emisor],
            Receptor: [that.$route.params.receptor]
          }
        })
        .then(function(response) {
          // handle success
          // Limpiamos el texto del input
          that.textoNuevoMensaje = "";
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
</style>

