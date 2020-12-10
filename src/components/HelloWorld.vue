<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <button @click="signin">try login</button>
    <button @click="refresh">try refresh</button>
    <button @click="logout">try logout</button>
  </div>
</template> 

<script>
import axios from "axios";
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      token: "",
    };
  },
  methods: {
    async signin() {
      let response = await axios.post(
        "http://localhost:8080/lasegunda-ml-api/login",
        {
          user: "prestadorml",
          password:
            "$2y$12$EZ/LseRRlWJ0.Yj0Y4g4HOwdxralvhWa.wIcOa86eES37ZIH2N.mW",
        }
      );
      this.token = response.data.token;
      console.log(response)
      console.log(response.headers['set-cookie'])
    },

    async refresh() {
      let response = await axios.post(
        "http://localhost:8080/lasegunda-ml-api/refreshToken",
        {},
        { headers: { Authorization: this.token },  withCredentials: true }
      );
      this.token = response.data.token;
      console.log(this.token)
      console.log(response.headers['set-cookie'])
    },

    logout() {
      axios.post(
        "http://localhost:8080/lasegunda-ml-api/logoutUser",
        {
          userId: 1753,
        },
        {
          headers: { Authorization: this.token },
          withCredentials: true,
        }
      );
    },
  },
};
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

