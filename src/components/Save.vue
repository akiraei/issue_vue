<template>
<div id="save">
<form>
    <p>
        from email
    <input v-model="fromEmail">
    </p>

    <p>
        to email
        <input v-model="toEmail">
    </p>

    <p>
        new?
        <input type="radio" id="new" value='new' v-model="newIssue">
        <label for="new">new</label>
        <input type="radio" id="not" value='notnew' v-model="newIssue">
        <label for="not">not</label>
    </p>

    <p v-show="newIssue === 'notnew'">
        issue num
        <input type="number" v-model="num">
    </p>

    <p>
        title
        <input v-model="title">
    </p>

    <p>
        content
        <textarea v-model="content"></textarea>
    </p>

    <p>
        due date
        <input v-model="dueDate">
    </p>

    <div>
       <h5>state</h5>
        <input type="radio" id="init" value='0' v-model="state">
        <label for="init">init</label>
        <input type="radio" id="check" value='1' v-model="state">
        <label for="check">check</label>
        <input type="radio" id="process" value='2' v-model="state">
        <label for="process">process</label>
        <input type="radio" id="done" value='3' v-model="state">
        <label for="done">done</label>
        <input type="radio" id="verified" value='4' v-model="state">
        <label for="verified">verified</label>
        <input type="radio" id="fin" value='5' v-model="state">
        <label for="fin">fin/closed</label>
        <input type="radio" id="unable" value='9' v-model="state">
        <label for="unable">unable/abort</label>
    </div>

    <p>
        priority
        <input type="number" min="0" max="9" v-model="priority">
    </p>

    <button v-on:click="send" v-show="ableCall">send</button>

</form>
</div>
</template>

<script>
import axios from "axios";
import moment from "moment";

export default {
  name: "save",
  data() {
    return {
      fromEmail: "",
      toEmail: "",
      num: "",
      title: "",
      content: "",
      dueDate: "",
      priority: 0,
      newIssue: "notnew",
      state: 0,
      ableCall: true
    };
  },
  props: {
    msg: String
  },
  methods: {
    send: async function() {
      this.$data.ableCall = false;

      let payload = { ...Object.assign(this.$data) };
      payload.logDate = moment().format("X");
      payload.dueDate = moment(payload.dueDate).format(`YYYY-MM-DD`);
      delete payload.ableCall;

      await axios
        .get("http://localhost:3000/highest-num")
        .then(res => {
          if (payload.newIssue === "new") {
            payload.num = parseInt(res.data) + 1;
          }
          delete payload.newIssue;
          axios.post("http://localhost:3000/save", payload).then(() => {
            console.log("send");
            this.$data.ableCall = true;
          });
        })
        .catch(() => {
          console.log("send err");
          this.$data.ableCall = true;
        });
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
form {
  margin-top: 40px;
}
</style>
