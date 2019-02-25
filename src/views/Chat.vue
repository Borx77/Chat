<template>
  <div class="chat">
    <h2 class="text-center">Chat</h2>
    <div class="bg-green-dark">cre</div>
  </div>
</template>

<script>
export default {
  name: "chat",
  props: ["emisor", "receptor"],
  components: {},
  mounted: function() {
    //$route.params.id

    //  obtenemos los datos de airtable
    let that = this;
    axios
      .get(
        "https://api.airtable.com/v0/appnLtER6XsQjN5RQ/Mensajes?view=Grid%20view"
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
</style>

