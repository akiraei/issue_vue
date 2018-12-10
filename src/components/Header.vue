<template>
    <div id="headery">
        <h2>{{page}}</h2>
        <span @click="find">find</span>
        <span @click="save">save</span>
    </div>
</template>
<script>
export default {
  name: "headery",
  props: {
    mutate: Object,
    navigate: String
  },
  computed: {
    page: function() {
      return this.$props.navigate;
    }
  },
  methods: {
    find: function() {
      this.$props.mutate.navigate("find");
    },
    save: function() {
      this.$props.mutate.navigate("save");
    }
  }
};
</script>
<style scoped lang="scss">
#headery {
  position: fixed;
  top: 0px;
  left: 0px;
  display: flex;
  width: 100vw;
  height: 40px;
  background-color: #999999;
  & * + * {
    margin-left: 20px;
  }
}
</style>
