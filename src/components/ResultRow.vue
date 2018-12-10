<template>
<tr>
  <td>
   {{result.num}}
  </td>
 <td>
   {{result.title}}
 </td>
 <td>
  {{result.toEmail}}
 </td>
    <td>
     {{result.fromEmail}}
    </td>
    <td>
     {{result.priority}}
    </td>
    <td>
     {{result.dueDate}}
    </td>
    <td>
     {{result.content}}
    </td>
    <td>
     {{result.state}}
    </td>
    <td>
     {{logResult}}
    </td>
</tr>
</template>
<script>
 import moment from "moment"

export default {
  name: "ResultRow",
  props: {
    result: Object
  },
  computed: {
    logResult: function() {
      return moment.unix(this.$props.result.logDate).format("YYYY-MM-DD hh:mm:ss a");
    }
  }
};
</script>
<style scoped lang="scss">
</style>
