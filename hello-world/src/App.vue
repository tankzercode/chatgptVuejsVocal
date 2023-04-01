<template>
  <div id="app">
    <button @click="listen">speak</button>
  </div>
</template>

<script>
import { Configuration, OpenAIApi } from "openai";

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
      conf: false,
      openai: false,
      response: false,
      reponsetext: false,
      reponseduchat: false,
    };
  },
  methods: {
    async initgpt() {
      this.conf = new Configuration({
        organization: "org-3EzPJjL2pbiVK4bPCkwADi1m",
        apiKey: "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",
      });

      this.openai = new OpenAIApi(this.conf);
      this.response = await this.openai.listEngines();
    },

    listen() {
      this.reco.start();

      this.reco.onresult = (event) => {
        const color = event.results[0][0].transcript;
        this.text = color;
        console.log(this.text);
        this.initgpt();
        this.askGpt(color);
      };
    },

    async askGpt(color) {
      this.reponsetext = await this.openai.createCompletion({
        model: "text-davinci-003",
        prompt: color,
        max_tokens: 120,
        temperature: 1,
      });
      console.log(this.reponsetext.data.choices[0].text);
      this.reponseduchat = this.reponsetext.data.choices[0].text;
      this.speak();
    },

    async speak() {
      let utterance = await new SpeechSynthesisUtterance(this.reponseduchat);
        synth.speak(utterance);
    }
  },

  mounted() {
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
