<template>
  <div id="app">
    <img width="25%" src="./assets/logo.png">
    <Page/>
    <!-- <User/> -->
    <!-- <UserComponent/> -->
    <!-- <UserImport/> -->
    <!-- <Home/> -->
  </div>
</template>

<script>
import Page from "./view/Page";
// import User from "./view/User";
// import UserComponent from "./view/UserComponent";
// import UserImport from "./view/UserImport";
// import Home from "./view/Home";

export default {
  name: "App",
  components: {
    Page
    // User,
    // UserComponent
    // UserImport
    // Home
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
