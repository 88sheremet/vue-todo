<template>
  <div class="weather">
    <div class="container">
      <div class="card">
        <input
          class="card__input"
          type="text"
          placeholder="input city"
          v-model="searchQuery"
          @keyup="weatherSearch"
        />
        <button class="search" @click="weatherSearch">Search</button>
      </div>

      <div class="card-load" v-if="loading">Loading</div>

      <div class="card-info" v-show="!error && location && temperature != 0 && description">
        <div class="card" v-show="error">Error</div>
        <div class="card-info__text">
          <p class="city">{{ location }}</p>
          <p class="temp">{{ temperature }} C</p>
          <p class="city">{{ description }}</p>
        </div>
      </div>
    </div>

    <div class="weather-bg">
      <div class="weather-bg">
        <img src="" alt="" />
        <img src="" alt="" />
        <img src="" alt="" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      location: '',
      temperature: 0,
      description: '',
      loading: false,
      error: false,
      searchQuery: '',
    }
  },
  methods: {
    weatherSearch() {
      this.loading = true
      this.error = false
      fetch(
        `http://api.weatherapi.com/v1/current.json?key=8b60d3e7b347463bab782028250605&q=${this.searchQuery}`
      )
        .then((response) => response.json())
        .then((data) => {
          this.loading = false
          this.location = data.location.name;
          this.temperature = data.current.temp_c;
          (this.description = data.current.condition.text), this.resetSearchQuery()
        })
        .catch((error) => {(this.loading = false), (this.error = true), console.error(error)})
    },
    resetSearchQuery() {},
  },
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Karla:ital,wght@0,200..800;1,200..800&family=League+Spartan:wght@100..900&display=swap');

:root {
  --accent: #12c0dd;
  --accent-rgb: 18, 192, 221;
}
body {
  font-family: 'Karla', sans-serif;
  height: 100%;
}
* {
  margin: 0;
  padding: 0;
}
body,
html {
  height: 100vh;
}
input,
button {
  font-family: 'Karla', sans-serif;
}
#app {
  height: 100%;
}
.weather {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  position: relative;
  overflow: hidden;
}
.container {
  width: 100%;
  max-width: 1000px;
  margin: 0 auto;
  padding-left: 15px;
  padding-right: 15px;
  display: grid;
  grid-template-columns: 1fr 100px 200px;
  gap: 20px;
  box-sizing: border-box;
}
</style>
