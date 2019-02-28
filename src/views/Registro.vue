<template>
  <div class="registro fuente">
    <h1 class="text-center pb-1">Accede al chat</h1>
    <div class="fondo flex justify-center pt-24">
      <div class="flex-col">
        <div class="pb-8">
          <button
            @click="verRegistro = !verRegistro"
            class="bg-white px-8 py-4 border rounded"
          >Regístrate</button>
        </div>
        <div v-if="!verRegistro">
          <form action="#formulario-contacto" method="post" id="formulario-contacto">
            <div>
              <input type="text" name="user" id="user" placeholder="Usuario">
            </div>
            <div>
              <input type="password" name="password" id="password" placeholder="Contraseña">
            </div>
            <div>
              <input type="submit" name="submit" value="Regístrate" class="boton">
            </div>
          </form>
        </div>
        <div class="text-center">
          <button class="bg-white px-10 py-4 border rounded">Entrar</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "registro",
  components: {},
  data: function() {
    return {
      verRegistro: "false"
    };
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

