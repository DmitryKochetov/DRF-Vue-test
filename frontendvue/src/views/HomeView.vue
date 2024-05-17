<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <div v-for="item in users" :key="item.id">
      {{ item.id + " " + item.username }}
    </div>
    <HelloWorld :msg="this.token" />
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";
import axios from "axios";

export default {
  name: "HomeView",
  components: {
    HelloWorld,
  },
  data() {
    return {
      token: 0,
      headers: 0,
      users: [],
    };
  },

  beforeMount() {
    console.log("hi");

    // axios
    //   .post("http://127.0.0.1:8000/api-auth-token/", {
    //     username: "dima",
    //     password: "123456",
    //   })
    //   .then((response) => {
    //     this.token = response.data.token;
    //     console.log("token:", this.token);
    //     localStorage.setItem("token", this.token);
    //   })
    //   .catch((error) => console.log(error));

    fetch("http://127.0.0.1:8000/api-auth-token/", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        username: "dima",
        password: "123456",
      }),
    })
      .then((response) =>  response.json())
      .then((data) => this.token = data.token)
      .catch((error) => console.error(error));

    axios
      .get(
        "http://127.0.0.1:8000/api/users/",
        "Authorization: Token " + this.token
      )
      .then((response) => {
        this.users = response.data;
      })
    .catch((error) => console.log(error));

  },
};
</script>
