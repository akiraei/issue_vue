<template>
<div>
<form>
    <p>
        from email
    <input v-model="fromEmail">
    </p>

    <p>
        to email
        <input v-model="toEmail">
    </p>


    <p >
        issue num
        <input type="number" v-model="num">
    </p>

    <p>
        title
        <input v-model="title">
    </p>


    <p>
        log date
        <input v-model="logDate.gte">
        <input v-model="logDate.lte" >
    </p>


    <p>
        due date
        <input v-model="dueDate.gte">
        <input v-model="dueDate.lte">
    </p>

    <p>
       <span>state</span>
        <select v-model="state.gte">
            <option value="0">select</option>
            <option value="1">init</option>
            <option value="2">check</option>
            <option value="3">process</option>
            <option value="4">done</option>
            <option value="5">verified</option>
            <option value="6">fin/closed</option>
            <option value="9">unable/abort</option>
        </select>
        <select v-model="state.lte">
            <option value="0">select</option>
            <option value="1">init</option>
            <option value="2">check</option>
            <option value="3">process</option>
            <option value="4">done</option>
            <option value="5">verified</option>
            <option value="6">fin/closed</option>
            <option value="9">unable/abort</option>
        </select>
    </p>

    <p>
        priority
        <input type="number" min="1" max="9" v-model="priority.gte">
        <input type="number" min="1" max="9" v-model="priority.lte">
    </p>

    <button v-on:click="send" v-show="ableCall">send</button>

</form>

    <div v-if="receiveSource">
        <table>
        <tr>
            <th>issue num</th>
            <th>title</th>
            <th>to</th>
            <th>from</th>
            <th>priority</th>
            <th>due date</th>
            <th>content</th>
            <th>state</th>
            <th>log date</th>
        </tr>
    <ResultRow v-for="item in resultArr" :key="item._id" :result="item"/>
        </table>
    </div>

</div>
</template>

<script>
import axios from "axios";
import moment from "moment";
import ResultRow from "./ResultRow";

export default {
  name: "find",
  computed: {
    receiveSource: function() {
      return this.$props.source.length > 0;
    },
    resultArr: function() {
      return this.$props.source;
    }
  },
  components: {
    ResultRow
  },
  data() {
    return {
      fromEmail: "",
      toEmail: "",
      num: "",
      title: "",
      logDate: {
        gte: "",
        lte: ""
      },
      dueDate: {
        gte: "",
        lte: ""
      },
      priority: {
        gte: 0,
        lte: 0
      },
      state: {
        gte: 0,
        lte: 0
      },
      ableCall: true
    };
  },
  props: {
    source: Array,
    mutate: Object
  },
  methods: {
    send: async function() {
      this.$data.ableCall = false;

      let payload = {};
      let obj = Object.assign(this.$data);
      Object.keys(obj).forEach(key => {
        if (obj[key]) {
          payload[key] = obj[key];
        }
      });

      if (payload.logDate.gte && payload.logDate.lte) {
        payload.logDate.gte = moment(payload.logDate.gte).format("X");
        payload.logDate.lte = moment(payload.logDate.lte).format("X");
      } else {
        delete payload.logDate;
      }
      if (payload.dueDate.gte && payload.dueDate.lte) {
        payload.dueDate.gte = moment(payload.dueDate.gte).format("YYYY-MM-DD");
        payload.dueDate.lte = moment(payload.dueDate.lte).format("YYYY-MM-DD");
      } else {
        delete payload.dueDate;
      }
      if (!payload.priority.gte || !payload.priority.lte) {
        delete payload.priority;
      }
      if (!payload.state.get || !payload.state.lte) {
        delete payload.state;
      } else {
        if (payload.state.lte === 9 || payload.state.gte === 9) {
          payload.state.gte = 9;
          payload.state.lte = 9;
        }
      }

      delete payload.ableCall;

      axios
        .post("http://localhost:3000/find", payload)
        .then(res => {
          console.log("send");

          let arr = res.data.map(e => {
            return {
              ...e
              // logDate: moment(res.data.logDate).format("YYYY-MM-DD hh:mm:ss a")
            };
          });

          // console.log("find rslt", arr);
          this.$props.mutate.source(arr);
          this.$data.ableCall = true;
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
  display: flex;
  height: 70px;
  position: fixed;
  left: 0px;
  top: 40px;
  & p {
    width: 100px;
    & input {
      width: 100px;
    }
  }
  & p + p {
    margin-left: 10px;
  }
  & button {
    margin-left: 10px;
    width: 50px;
  }
}
table {
  margin-top: calc(40px + 70px);
  width: 100vw;
    text-align: center;
}
</style>
