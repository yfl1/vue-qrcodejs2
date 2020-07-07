<template>
  <div id="app">
    <qrcode></qrcode>
    <button @click="show=!show">generate a new qrcode</button>
    <qrcode v-if="show"></qrcode>
  </div>
</template>

<script>
import qrcode from "./components/qrcode.vue";
export default {
  name: "App",
  components: {
    qrcode
  },
  data: function() {
    return {
      show: false
    };
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
