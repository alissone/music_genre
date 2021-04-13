<template>
  <div class="hello">
    <div class="container">
      <button class="button" v-if="!micError" @click="toggleRecording">
        <i id="mic" :class="isRecordingClass"></i>
      </button>
      <p v-if="micError">I don't have permission to use the microphone 😢</p>
      <button class="btn btn-default" id="start">Start</button>
      <button class="btn btn-default" id="stop">Stop</button>
      <div>
        <ul class="list-unstyled" id="ul"></ul>

        <a :href="downloadUrl">Download here</a>
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
      stream: null,
      recorder: null,
      chunks: [],
      mediaInfo: {
        type: "audio/ogg",
        ext: ".ogg",
      },
      count: 0,
      downloadUrl: "",
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
    async initializeRecorder() {
      try {
        this.stream = await navigator.mediaDevices.getUserMedia({
          audio: true,
        });
        this.recorder = new MediaRecorder(this.stream);
      } catch (error) {
        if (error.name == "NotAllowedError") {
          this.micError = true;
        }
      }
      console.log("Recorder outside");
      console.log(this.recorder);
    },
    async toggleRecording() {
      console.log("triggering recording");
      if (this.recorder == null) await this.initializeRecorder();

      console.log("this recorder from toggleRecording");
      console.log(this.recorder);

      if (!this.isRecording) {
        this.isRecording = true;
        this.chunks = [];
        this.recorder.start();

        console.log("this recorder after starting:");
        console.log(this.recorder);
        return;
      }

      if (this.isRecording) {
        this.recorder.stop();
        this.isRecording = false;
        this.recorder.ondataavailable = (e) => {
          this.chunks.push(e.data);
          if (this.recorder.state == "inactive") this.makeLink();
        };
        return;
      }
    },
    makeLink() {
      let blob = new Blob(this.chunks, { type: this.mediaInfo.type });
      this.downloadUrl = URL.createObjectURL(blob);

      // convert blob to base64
      var reader = new FileReader();
      reader.readAsDataURL(blob);
      reader.onloadend = function () {
        var base64data = reader.result;
        console.log(base64data);
      };

      // download the file
      // var a = document.createElement("a");
      // a.href = this.downloadUrl;
      // a.target = "_blank";
      // a.download = "lkn_" + this.count + ".ogg";
      // document.body.appendChild(a);
      // a.click();

      console.log("this.downloadUrl", this.downloadUrl);
    },
  },
};
</script>

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
