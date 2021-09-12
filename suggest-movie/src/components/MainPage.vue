<template>
  <div class="main">
    <div class="language">
      <a
        v-if="defaultLanguage == 'tr'"
        style="color: red"
        @click="setLanguage('tr')"
        >TR</a
      >
      <a v-else @click="setLanguage('tr')">TR</a>
      &nbsp;
      <a
        v-if="defaultLanguage == 'en'"
        style="color: red"
        @click="setLanguage('en')"
        >EN</a
      >
      <a v-else @click="setLanguage('en')">EN</a>
    </div>

    <div class="trLang" v-if="defaultLanguage == 'tr'">
      <h1>Anasayfa</h1>
      <button @click="getList">Film Getir</button>
      <div class="result" v-if="Object.keys(result).length > 0">
        <h3>İsim: {{ result.title }}</h3>
        <h3>Yıl: {{ result.year }}</h3>
        <h3>Imdb Puanı: {{ result.imDbRating }}</h3>
        <h3>Ekip: {{ result.crew }}</h3>
        <img :src="result.image" alt="{{ result.fullTitle }}" />
      </div>
    </div>

    <div class="enLang" v-else>
      <h1>Main Page</h1>
      <button @click="getList">Get Movie</button>

      <div class="result" v-if="Object.keys(result).length > 0">
        <h3>Name: {{ result.title }}</h3>
        <h3>Year: {{ result.year }}</h3>
        <h3>Imdb Rating: {{ result.imDbRating }}</h3>
        <h3>Crew: {{ result.crew }}</h3>
        <img :src="result.image" alt="{{ result.fullTitle }}" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "MainPage",

  data() {
    return {
      api_key: "",
      result: {},
      defaultLanguage: localStorage.getItem("defaultLanguage")
        ? localStorage.getItem("defaultLanguage")
        : "en",
    };
  },

  created() {
    this.setLanguage(this.defaultLanguage);
  },

  methods: {
    setLanguage(language) {
      this.defaultLanguage = language;
      localStorage.setItem("defaultLanguage", this.defaultLanguage);
    },

    randomMovie(datas) {
      return datas[Math.floor(Math.random() * datas.length)];
    },

    getList() {
      let url = "https://imdb-api.com/en/API/Top250Movies/" + api_key;
      axios.get(url).then((response) => {
        this.result = this.randomMovie(response.data.items);
      });
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
a {
  cursor: pointer;
  font-weight: 700;
  color: black;
}
a:hover {
  font-size: 20px;
  color: red;
}
</style>
