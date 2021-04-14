<template>
  <div id="app">
    <link
      rel="stylesheet"
      href="https://pro.fontawesome.com/releases/v5.10.0/css/all.css"
      integrity="sha384-AYmEC3Yw5cVb3ZcuHtOA93w35dYTsvhLPVnYs9eStHfGJvOvKxVfELGroGkvsg+p"
      crossorigin="anonymous"
    />
    <link rel="preconnect" href="https://fonts.gstatic.com" />
    <link
      href="https://fonts.googleapis.com/css2?family=Raleway&display=swap"
      rel="stylesheet"
    />

    <div class="container instruction-text">
      Click this button to record a song<br />
      We will try to guess what genre it is
    </div>
    <record-button v-model="audioContentBase64" />
    <p v-if="audioContentBase64 != ''">Base64 text:</p>
    <p>{{ audioContentBase64 }}</p>
  </div>
</template>

<script>
import RecordButton from "./components/RecordButton.vue";

export default {
  name: "App",
  data() {
    return {
      audioContentBase64: "",
    };
  },
  components: {
    RecordButton,
  },
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

.instruction-text {
  font-size: 2em;
  color: #333333;
  font-family: "Raleway", sans-serif;
  text-align: center;
}
</style>
