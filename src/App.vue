<template>
  <div id="app">
    <drag v-if="this.api_key" :api_key="this.api_key" />
    <div class="login" v-if="!this.api_key">
      <div class="login-text">
        Для дальнейшей работы приложения необходимо авторизоваться в VK!
      </div>
      <b-button
        variant="primary"
        href="https://oauth.vk.com/authorize?client_id=7791417&scope=friends,offline&display=page&redirect_uri=egormalyshev99.github.io/wgg-test-task/&response_type=token&v=5.130"
        >Авторизоваться</b-button
      >
    </div>
  </div>
</template>

<script>
import Drag from "./components/Drag";

export default {
  name: "App",
  components: {
    Drag,
  },
  data() {
    return {
      api_key: null,
    };
  },
  mounted() {
    const access_token = window.location.hash.replace("#access_token=", "");

    if (access_token) {
      this.api_key = access_token;
    }
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}

* {
  box-sizing: border-box;
}

body {
  background-color: rgb(34, 34, 38) !important;
}

.login {
  margin-top: 15%;
  color: #fff;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.login-text {
  margin-bottom: 20px;
}
</style>
