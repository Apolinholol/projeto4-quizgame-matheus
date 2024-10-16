<template>
  <div>
    
  </div>
</template>

<script lang="ts">

import axios from 'axios';
import { defineComponent } from 'vue';


export default defineComponent({
  name: 'App',
  created() {
    axios
      .get('https://opentdb.com/api.php?amount=10')
      .then(response => console.log(response.data))
  },

  });
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
