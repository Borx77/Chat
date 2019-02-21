<template>
  <div class="list">
    <router-link to="/">Home</router-link>
    <div class="w-full bg-grey-dark py-6">
      <h1 class="text-center text-white">Chat de Amigos</h1>
    </div>
    <div class="bg-white-light w-full flex justify-center">
      <div type="text" class="w-5/6 bg-grey-lighter m-3 p-2">
        <i class="fas fa-search"></i>
        <input>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";

export default {
  name: "list",
  components: {}
};
</script>

<style lang="postcss" scoped>
</style>

