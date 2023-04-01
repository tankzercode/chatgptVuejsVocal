<template>
  <div id="app">
    <button @click="listen">speak</button>
  </div>
</template>

<script>
const recognition = window.SpeechRecognition || window.webkitSpeechRecognition;
const synth = window.speechSynthesis;

export default {
  name: "App",
  components: {},

  data() {
    return {
      recognition: recognition,
      reco: "",
      parole: "",
      text: "",
      voices: synth.getVoices(),
    };
  },
  methods: {
    listen() {
      this.reco.start();

      this.reco.onresult = (event) => {
        const color = event.results[0][0].transcript;
        this.text = color;
        console.log(this.text);

        let utterance = new SpeechSynthesisUtterance(this.text);
        synth.speak(utterance);
      };
    },
  },

  mounted() {
    console.log(this.recognition);
    this.reco = new recognition();

    this.parole = synth;
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
</style>
