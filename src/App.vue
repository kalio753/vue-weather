<template>
  <div
    id="app"
    :class="
      typeof weather.main !== undefined &&
      (weather?.main?.temp / 10).toFixed(0) > 16
        ? 'warm'
        : ''
    "
  >
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          v-model="query"
          @keypress="fetchWeather"
        />
        <div v-if="error" class="error">
          {{ error }}
        </div>
      </div>

      <div v-if="Object.keys(weather).length > 0" class="weather-wrap">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>

          <div v-if="date" class="date">{{ date }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ (weather.main.temp / 10).toFixed(0) }}°C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
      <div v-else class="hint">Please search for a country / city</div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      api_key: "85c1e47737daf6818c7a4df3b55756ca",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
      error: "",
      date: ""
    }
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        if (this.query === "") {
          this.weather = {}
          this.date = ""
        } else {
          fetch(`${this.url_base}weather?q=${this.query}&appid=${this.api_key}`)
            .then((res) => res.json())
            .then((res) => {
              if (res.cod == 200) {
                var options = {
                  weekday: "long",
                  year: "numeric",
                  month: "long",
                  day: "numeric"
                }
                this.weather = res
                this.date = new Date().toLocaleDateString("en-US", options)
                this.error = ""
              } else {
                this.error = "Cannot find " + this.query
              }
            })
        }
      }
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  /* font-family: "montserrat", san-serif; */
  font-family: "Montserrat";
}

#app {
  background-image: url("./assets/cold-bg.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm {
  background-image: url("./assets/warm-bg.jpg");
}

main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0 0 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0 16px 0 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0 0 16px rgba(0, 0, 0, 0.5);
  background: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0 16px 0;
}

.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 600;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.3);
}

.location-box .date {
  color: #fff;
  font-size: 19px;
  font-weight: 300;
  text-align: center;
  font-style: italic;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.3);
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.3);
  background-color: rgba(0, 0, 0, 0.3);
  border-radius: 20px;
  margin: 30px 0;
  box-shadow: 4px 6px rgba(0, 0, 0, 0.4);
}

.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 600;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.3);
}

.error {
  font-size: 12px;
  font-style: italic;
  color: red;
  font-weight: 400;
  margin: 12px 8px;
  text-shadow: -0.5px -0.5px 0 #000, 0.5px -0.5px 0 #000, -0.5px 0.5px 0 #000,
    0.5px 0.5px 0 #000;
}

.hint {
  text-align: center;
  color: #fff;
  font-size: 16px;
  font-weight: 400;
  text-shadow: 2px 2px rgba(0, 0, 0, 0.3);
}
</style>
