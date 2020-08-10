<template>
  <div>
    <NavBar />
    <div class="container"></div>
    <router-view />
  </div>
</template>

<script>
import NavBar from "./components/NavBar";
import "../node_modules/materialize-css/dist/css/materialize.min.css";
import "../node_modules/materialize-css/dist/js/materialize.min.js";

export default {
  name: "App",
  components: {
    NavBar,
  },
};
</script>

<style></style>
