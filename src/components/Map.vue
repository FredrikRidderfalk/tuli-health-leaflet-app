<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p class="subtitle">Locate your area</p>
    <label for="post-code">Enter your post code here: </label>
    <input id="post-code" />
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
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
