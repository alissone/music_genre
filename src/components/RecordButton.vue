<template>
  <div class="hello">
    <div class="container">
      <button class="button" v-if="!micError" @click="toggleRecording">
        <i id="mic" :class="isRecordingClass"></i>
      </button>
      <p v-if="micError">I don't have permission to use the microphone 😢</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "RecordButton",
  props: {
    value: String,
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
    },

    startRecording() {
      this.isRecording = true;
      this.chunks = [];
      this.recorder.start();
    },

    stopRecording() {
      this.recorder.stop();
      this.isRecording = false;
      this.recorder.ondataavailable = (e) => {
        this.chunks.push(e.data);
        if (this.recorder.state == "inactive") this.makeLink();
      };
    },

    async toggleRecording() {
      if (this.recorder == null) await this.initializeRecorder();

      if (!this.isRecording) {
        this.startRecording();
        return;
      }

      if (this.isRecording && this.recorder.state != "inactive") {
        this.stopRecording();
        return;
      }
    },

    async makeLink() {
      let blob = new Blob(this.chunks, { type: this.mediaInfo.type });
      this.downloadUrl = URL.createObjectURL(blob);

      // Convert blob to base64
      var data = "";
      var reader = new FileReader();
      reader.readAsDataURL(blob);

      const base64audio = await new Promise((resolve, reject) => {
        reader.onloadend = function (event) {
          resolve(reader.result);
        };
      });

      this.$emit("input", base64audio);
      this.$emit("link", this.downloadUrl);

      // Download the file
      // var a = document.createElement("a");
      // a.href = this.downloadUrl;
      // a.target = "_blank";
      // a.download = "lkn_" + this.count + ".ogg";
      // document.body.appendChild(a);
      // a.click();
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
