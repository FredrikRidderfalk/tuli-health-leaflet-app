<template>
  <div class="container">
    <h1 class="title">{{ msg }}</h1>
    <p class="subtitle">Locate your area</p>
  </div>
</template>

<script>
export default {
  name: 'Map',
  props: {
    msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .title {
    margin-bottom: 2em;
  }

  .subtitle {
    font-weight: 600;
    margin-bottom: 1em;
  }

  .container {
    margin-bottom: 2em;
  }
</style>
