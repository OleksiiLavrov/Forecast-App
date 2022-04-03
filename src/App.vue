<template>
  <section class="app" :class="{ warm: warmTheme }">
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrap" v-if="showWeather">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div class="date">{{ dateBuilder }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </section>
</template>

<script>
export default {
  name: "App",
  components: {},
  data() {
    return {
      api_key: "85234ef3e426d0b1d7a7547a51851f69",
      url_base: "http://api.openweathermap.org/data/2.5/",
      query: "",
      showWeather: false,
      warmTheme: false,
      weather: {},
    };
  },
  computed: {
    dateBuilder: function () {
      let d = new Date();
      const months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      const days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    },
  },
  methods: {
    check() {
      if (!this.weather.main) {
        this.showWeather = true;
      }
    },
    async fetchWeather(e) {
      if (e.key == "Enter") {
        const response = await fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
        );
        const weatherNow = await response.json();
        this.check();
        this.weather = weatherNow;
        this.warmTheme = this.weather.main.temp > 16;
      }

      if (e.key !== "Enter") return;
    },
  },
};
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  margin: 0 auto;
  font-family: "montserrat", sans-serif;
  max-width: 500px;
}

.app {
  background-image: url(./assets/bg-1.jpg);
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

.app.warm {
  background-image: url(./assets/bg-2.jpg);
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

  background-color: rgba(255, 255, 255, 0.5);

  box-shadow: 0 0 8px rgba(0, 0, 0, 0.25);
  border-radius: 0 16px 0 16px;
  transition: 0.4s;

  &::placeholder {
    color: #505050;
  }

  &:focus {
    box-shadow: 0 0 16px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.75);
    border-radius: 16px 0 16px 0;
  }
}

.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box {
  text-align: center;

  .temp {
    display: inline-block;
    padding: 10px 25px;
    color: #fff;
    font-size: 102px;
    font-weight: 900;

    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.25);
    border-radius: 30px;
    margin: 30px 0;

    box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  }

  .weather {
    color: #fff;
    font-size: 48px;
    font-weight: 700;
    font-style: italic;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  }
}
</style>
