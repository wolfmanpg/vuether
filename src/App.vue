<template>
  <div :class="mainDivObject">
    <div class="min-h-screen p-6 md:p-10">
      <h1 class="text-white text-center text-2xl mb-5 font-semibold">
        Welcome to Vuether
      </h1>

      <search-bar
        @set-weather="setWeather"
        @show-weather="showWeather"
      ></search-bar>

      <div v-if="showWeatherInfo" class="weatherBox">
        <weather-box
          :location="location"
          :description="description"
          :temp="temp"
          :errorMessage="errorMessage"
        ></weather-box>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import SearchBar from "./components/SearchBar.vue";
import WeatherBox from "./components/WeatherBox.vue";

export default {
  components: {
    WeatherBox,
    SearchBar,
  },

  setup() {
    const location = ref("");
    const description = ref("");
    const errorMessage = ref("");
    const temp = ref(0);
    const showWeatherInfo = ref(false);
    const mainDivObject = ref({
      "background-section": true,
      rainy: false,
      sunny: false,
      errorbckg: false,
      general: true,
    });

    const setBackrgoundImage = () => {
      mainDivObject.value.rainy = false;
      mainDivObject.value.sunny = false;
      mainDivObject.value.general = false;
      mainDivObject.value.errorbckg = false;

      if (errorMessage.value !== ''){
         mainDivObject.value.errorbckg = true;
         return;
      }

      if (temp.value < 21 || description.value.includes('cloud')) {
        mainDivObject.value.rainy = true;
      } else if (temp.value > 21 && temp.value < 30) {
        mainDivObject.value.general = true;
      } else {
        mainDivObject.value.sunny = true;
      }
    };

    return {
      showWeatherInfo,
      location,
      description,
      temp,
      errorMessage,
      mainDivObject,
      setBackrgoundImage,

      showWeather(show) {
        showWeatherInfo.value = show;
      },

      setWeather(weatherObject) {
        location.value = weatherObject.location;
        description.value = weatherObject.description;
        temp.value = weatherObject.temp;
        errorMessage.value = weatherObject.errorMessage;

        setBackrgoundImage();
      },
    };
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Montserrat:wght@200;300&family=Nunito&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "Montserrat", sans-serif;
  font-family: "Nunito", sans-serif;
}

.background-section {
  background-size: cover;
  background-position: center;
}

.general {
  background-image: url("./assets/weather.jpg");
}

.rainy {
  background-image: url("./assets/rainy.jpg");
}

.sunny {
  background-image: url("./assets/sunny.jpg");
}

.errorbckg{
    background-image: url("./assets/notfound.jpg");
}
</style>
