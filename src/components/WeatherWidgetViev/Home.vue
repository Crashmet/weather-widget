<template :key="regions">
  <div v-if="regions.length > 0">
    <div v-for="(region, index) in regions" class="cards" :key="index">
      <div>
        <div class="grid-row">
          <h3 class="weather-city">
            {{ region.city }}
          </h3>
        </div>
        <div class="grid-row">
          <div class="weather-row">
            <i class="icon-weather"></i>
            <h1>{{ region.temperature }} °C</h1>
          </div>
        </div>
        <div class="grid-row">
          <div
            class="grid-item weather-description"
            style="flex-basis: 100%; margin-left: 20px"
          >
            <h3>{{ region.weather }} - {{ region.about_weather }}</h3>
          </div>
        </div>
        <div class="grid-row">
          <div class="grid-item weather-description">
            <i class="icon fa fa-compass" aria-hidden="true"></i>Wind:
            {{ region.wind_speed }}m\s
          </div>

          <div class="grid-item weather-description">
            <i class="icon fa fa-ravelry" aria-hidden="true"></i
            >{{ region.pressure }}hPa
          </div>

          <div class="grid-item weather-description">
            <p style="margin-left: 10px">
              Visibility: {{ region.visibility }}km.
            </p>
          </div>

          <div class="grid-item weather-description">
            <p style="margin-left: 10px">Humidity: {{ region.humidity }}%</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
export default {
  props: ['regions'],
};
</script>

<style scoped>
.weather-city {
  margin-left: 15px;
  margin-top: 10px;
  font-size: 20px;
  font-weight: 600;
}
.weather-row {
  display: flex;
  align-items: center;
}
.icon-weather {
  width: 100px;
  height: 100px;
  background: url('@/assets/cloudy-day-1.svg') no-repeat;
  background-size: contain;
}
.weather-description {
  margin-bottom: 10px;
  /* margin: 0 5px 10px 5px; */
}
</style>
