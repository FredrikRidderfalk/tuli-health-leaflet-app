<template>
  <div class="container">
    <h1 class="header">{{ msg }}</h1>
    <p class="subtitle">Locate your area</p>
    <label for="post-code">Enter your post code here: </label>
    <input id="post-code" placeholder="E.g. SW7 3DX" />
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
  .header {
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
