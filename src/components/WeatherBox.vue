<template>
  <div class="container mx-auto mt-20">
    <div class="flex flex-col text-center text-2xl font-semibold">
      <div
        v-if="errorMessage !== ''"
        class="error-box text-center mt-10 font-bold text-2xl rounded-3xl text-error p-14"
      >
        <p>{{ errorMessage }}</p>
        <p class="mt-5">Please try again..</p>
      </div>

      <div v-else>
        <div class="location-box">
          <p class="location-text text-white text-5xl font-semibold mb-3">{{ location }}</p>
          <p class="date-text text-white text-2xl">{{ today }}</p>
        </div>

        <div class="weather-status mt-5">
          <div class="temp-box pt-20 pb-20 pl-5 pr-5 md:p-16 lg:pt-20 lg:pb-20 rounded-3xl w-3/4 lg:w-1/2 xl:w-1/3 mx-auto mb-10">
            <p class="text-white text-5xl xl:text-6xl">{{ temp }} °C</p>
          </div>
          <p class="weather-desc text-white text-4xl italic uppercase">{{ description }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { computed } from "vue";

export default {
  props: {
    location: {
      type: String,
      required: true,
      default: "",
    },
    temp: {
      type: Number,
      required: true,
      default: 0,
    },
    description: {
      type: String,
      required: true,
      default: "",
    },
    errorMessage: {
      type: String,
      required: false,
      default: "",
    },
  },
  setup() {
    const today = computed(() => {
      const todayDate = new Date();
      const yyyy = todayDate.getFullYear();
      let mm = todayDate.getMonth() + 1; // Months start at 0!
      let dd = todayDate.getDate();

      if (dd < 10) dd = "0" + dd;
      if (mm < 10) mm = "0" + mm;

      return dd + "/" + mm + "/" + yyyy;
    });

    return {
      today,
    };
  },
};
</script>

<style scoped>

.location-box p, .weather-desc{
    text-shadow: 1px 4px rgba(0, 0, 0, 0.35);
}

.temp-box, .error-box{
  background-color: rgba(255, 255, 255, 0.45);
}

.temp-box p{
  text-shadow: 5px 5px 11px black;
}

</style>