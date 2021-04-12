<template>
  <div class="hello">
    <div class="container">
      <button class="button" v-if="!micError" @click="addToAmethod">
        <i id="mic" :class="isRecordingClass"></i>
      </button>
      <p v-if="micError">I don't have permission to use the microphone 😢</p>
      <button class="btn btn-default" id="start">Start</button>
      <button class="btn btn-default" id="stop">Stop</button>
      <div>
        <ul class="list-unstyled" id="ul"></ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "RecordButton",
  props: {
    msg: String,
  },
  data() {
    return {
      isRecording: false,
      micError: false,
    };
  },
  computed: {
    isRecordingClass: function () {
      var cls = "fas fa-microphone microphone grow";
      if (this.isRecording) cls += " is-recording";
      if (this.micError) cls += " transparent";
      return cls;
    },
  },
  methods: {
    startRecording() {
      console.log("Hello");
    },
    addToAmethod: function () {
      let constraints = { audio: true };
      navigator.mediaDevices
        .getUserMedia(constraints)
        .then((stream) => {
          console.log(stream);
          console.log("FUNCIONA");
        })
        .catch((err) => {
          if (err.name == "NotAllowedError") {
            this.micError = true;
          }
        });

      console.log("triggering recording");
      this.isRecording = !this.isRecording;
      console.info(this.isRecording);
    },
  },
};
</script>

<!--<script src="../scripts/record.js"></script> -->


<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
body {
  background: #e2e2e2;
  font-size: 62.5%;
}

.microphone {
  font-size: 4em;
  color: #d1d1d1;
}

.transparent {
  color: rgba(1, 1, 1, 0);
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
