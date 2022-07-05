<template>
  <div>
    <div class="w-full">
      <input
        class="w-full border-none rounded-xl shadow-xl outline-none appearance-none p-3 font-normal text-xl"
        placeholder="Search"
        type="text"
        name="searchBar"
        id="searchBar"
        v-model="searchValue"
        @keyup="fetchWeather"
      />
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import axios from "axios";

export default {
  emits: ["setWeather", "showWeather"],
  setup(_, { emit }) {
    const apiKey = ref("28ef54c00c3ed2f4475f4944795a8162");
    const apiUrl = ref("https://api.openweathermap.org/data/2.5/");
    const searchValue = ref("");
    const error = ref("");

    return {
      searchValue,
      error,

      async fetchWeather() {

        if (searchValue.value.length > 3) {
          //prepare the url 
          let url =
            apiUrl.value +
            "weather?q=" +
            searchValue.value +
            "&units=metric&APPID=" +
            apiKey.value;

          //make the get url  
          await axios
            .get(url)
            .then((response) => {
              error.value = '';
              let data = response.data;

              if (data) {
                emit("showWeather", true);

                emit("setWeather", {
                  location: data.name,
                  description: data.weather[0].description,
                  temp: data.main.temp,
                  errorMessage: ''
                });
              }
            })
            .catch((errorResponse) => {
                emit("setWeather", {
                  location: '',
                  description: '',
                  temp: 0,
                  errorMessage: errorResponse.response.data.message
                });
            });
        }
        else{
           emit("showWeather", false);
        }
      },
    };
  },
};
</script>

