<template>
  <div class="chat fuente">
    <Loading v-if="loading"></Loading>
    <h2 class="text-center my-2">Chat</h2>
    <div class="bg-green-dark w-full pb-5 flex">
      <div class="flex-0 tamanyo-div-titulo">
        <router-link :to="{ name: 'list' }">
          <i class="fas fa-caret-square-left tamanyo text-white border border-black mt-5"></i>
        </router-link>
      </div>
      <div class="flex-1">
        <h2 class="margin-titulo">{{ receptorUsername }}</h2>
      </div>
    </div>
    <div class="custom-scrollbar scrol">
      <div
        class="bg-grey-light w-2/3 mt-3 p-3 rounded"
        v-for="mensaje in mensajes"
        :key="mensaje.id"
        :class="{'ml-4': mensaje.fields.Receptor == $route.params.receptor, 'margin-chat': mensaje.fields.Receptor != $route.params.receptor}"
      >
        <p>
          <span
            :class="{'text-black': mensaje.fields.Receptor == $route.params.receptor, 'text-black': mensaje.fields.Receptor != $route.params.receptor}"
          >{{ mensaje.fields.Texto }}</span>
        </p>
      </div>
    </div>
    <div class="mt-3 fixed fixed pin-b pin-l p-5 w-full">
      <input type="text" class="border border-black tamanyo-input" v-model="textoNuevoMensaje">
      <button @click.prevent="nuevoMensajeapi" class="p-2 bg-green ml-2">Enviar</button>
    </div>
  </div>
</template>

<script>
import Loading from "@/components/Loading.vue";

export default {
  name: "chat",
  props: ["emisor", "receptor"],
  components: {
    Loading
  },
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
          if (response.data.records.length != that.mensajes.length) {
            that.mensajes = response.data.records;

            that.loading = false;
          }
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
  },
  updated: function() {
    let custom = document.querySelector(".custom-scrollbar");
    custom.scrollTo(0, document.body.scrollHeight);
    console.log("Hola");
  }
};
</script>

<style lang="postcss" scoped>
.scrol {
}
.tamanyo {
  font-size: 1.5rem;
}
.tamanyo-div-titulo {
  width: 44.5%;
}
.tamanyo-input {
  width: 13.5rem;
  margin-left: 0.35rem;
}
.fuente {
  font-family: "PT Sans Narrow", sans-serif;
}
.margin-chat {
  margin-left: 5.6rem;
}
.margin-titulo {
  padding-top: 1.2rem;
}
.custom-scrollbar {
  height: 70vh;
  overflow-y: scroll;
}

/* To style the document scrollbar, remove `.custom-scrollbar` */

.custom-scrollbar::-webkit-scrollbar {
  width: 9px;
}

.custom-scrollbar::-webkit-scrollbar-track {
  box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
  border-radius: 10px;
}

.custom-scrollbar::-webkit-scrollbar-thumb {
  border-radius: 10px;
  box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.5);
}
</style>

