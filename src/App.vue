<template>
  <div id="app">
    <Header :navigate="navigate" :mutate="mutate"/>
    <div v-if="navigate === 'find'">
      <Find :source="source" :mutate="mutate"/>
    </div>
    <div v-else>
      <Save />
    </div>
  </div>
</template>

<script>
import Find from "./components/Find.vue";
import Save from "./components/Save.vue";
import Header from "./components/Header.vue";

export default {
  name: "app",
  components: {
    Find,
    Save,
    Header
  },
  data() {
    const origin = this;
    return {
      source: [],
      navigate: "find",
      mutate: {
        source: function(obj) {
          origin.source = obj;
        },
        navigate: function(str) {
          origin.navigate = str;
        }
      }
    };
  }
};
</script>

<style lang="scss">
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
* {
  margin: 0px;
  padding: 0px;
  box-sizing: border-box;
}
</style>
