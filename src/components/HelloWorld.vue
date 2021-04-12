<template>
  <div class="hello">
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
    <div class="container">
      <button class="button" @click="addToAmethod">
        <i id="mic" :class="isRecordingClass"></i>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      isRecording: false,
    };
  },
  computed: {
    isRecordingClass: function () {
      if (this.isRecording) return "fas fa-microphone microphone grow";
      return "fas fa-microphone microphone grow is-recording";
    },
  },
  methods: {
    startRecording() {
      console.log("Hello");
    },
    addToAmethod: function () {
      console.log("triggering recording");
      this.isRecording = !this.isRecording;
      console.info(this.isRecording);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.instruction-text {
  font-size: 3em;
  color: #333333;
  font-family: "Raleway", sans-serif;
  text-align: center;
}

body {
  background: #e2e2e2;
  font-size: 62.5%;
}

.microphone {
  font-size: 4em;
  color: #d1d1d1;
}

.is-recording {
  color: #dc3545;
}

.container {
  padding: 2em;
}

/* GENERAL BUTTON STYLING */
button,
button::after {
  -webkit-transition: all 0.5s;
  -moz-transition: all 0.5s;
  -o-transition: all 0.5s;
  transition: all 0.5s;
}

button {
  background: none;
  border: 0px solid #d1d1d1;
  border-radius: 5px;
  color: #fff;
  display: block;
  font-size: 1.6em;
  font-weight: bold;
  margin: 1em auto;
  padding: 2em 6em;
  position: relative;
  text-transform: uppercase;
}

.grow {
  transition: all 0.2s ease-in-out;
}

.button:hover #mic {
  transform: scale(1.1);
}

.button {
  border-radius: 150px;
  background: linear-gradient(145deg, #fbfbfb, #d4d4d4);
  box-shadow: 30px 30px 60px #c8c8c8, -30px -30px 60px #ffffff;
}

.button:hover {
  border-radius: 150px;
  background: #ebebeb;
  box-shadow: 30px 30px 60px #c9c9c9, -30px -30px 60px #ffffff;
  border: 1px solid #d1d1d1;
}

.button:active {
  border-radius: 150px;
  background: linear-gradient(145deg, #d4d4d4, #fbfbfb);
  box-shadow: 30px 30px 60px #c8c8c8, -30px -30px 60px #ffffff;
}
</style>
