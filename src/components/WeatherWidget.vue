<template>
  <div class="container">
    <div class="toolbar grid-row">
      <i
        v-on:click="editWidget"
        class="grid-item gear fa fa-sliders"
        aria-hidden="true"
      ></i>
    </div>

    <Home
      v-if="!editMode"
      v-bind:regions="regions"
      v-bind:addCustomRegion="addCustomRegion"
      v-bind:deleteRegion="deleteRegion"
      v-bind:saveRegions="saveRegions"
    />

    <Options
      v-else
      v-bind:regions="regions"
      v-bind:addCustomRegion="addCustomRegion"
      v-bind:deleteRegion="deleteRegion"
      v-bind:saveRegions="saveRegions"
    />
  </div>
</template>

<script lang="ts">
import Home from '@/components/WeatherWidgetViev/Home.vue';
import Options from '@/components/WeatherWidgetViev/Options.vue';

import { RegionObject } from '@/interface/RegionObject';
import { defineComponent } from 'vue';

export default defineComponent({
  components: { Home, Options },

  data() {
    return {
      regions: new Array<RegionObject>() || null,
      editMode: true,
      test: 'test123',
      lat: 0,
      lon: 0,
      appid: '0046f25cba5aaac4500df334ec0aa5e3',
    };
  },

  mounted() {
    this.loadRegions();
    this.editWidget();
  },

  methods: {
    addLocalRegion: function (): void {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition((position) => {
          this.lat = position.coords.latitude;
          this.lon = position.coords.longitude;

          const region = new RegionObject();
          region
            .getAndMapDataByPosition(this.lat, this.lon, this.appid)
            .then(() => {
              this.regions.push(region);
              region.id = this.regions.length;
              this.saveRegions();
            });
        });
      }
    },

    addCustomRegion: function (city: string): void {
      const region = new RegionObject();

      region.getAndMapDataByCity(city, this.appid).then(() => {
        this.regions.push(region);
        region.id = this.regions.length;
        this.saveRegions();
      });
    },

    editWidget: function (): void {
      this.editMode = !this.editMode;
    },

    saveRegions: function (): void {
      let data: any;

      data = this.regions.map((item) => {
        return item.city;
      });

      data = [...new Set(data)];
      data = JSON.stringify(data);
      localStorage.setItem('regions', data);
    },

    loadRegions: function (): void {
      const data: any = localStorage.getItem('regions');
      const loadedRegions = JSON.parse(data);

      if (loadedRegions == null || loadedRegions.length == 0) {
        this.addLocalRegion();
      } else {
        loadedRegions.forEach((item: string) => {
          this.addCustomRegion(item);
        });
      }
    },

    deleteRegion: function (id: number): void {
      this.regions = this.regions.filter((region) => {
        return region.id != id;
      });

      this.saveRegions();
    },
  },
});
</script>

<style scoped>
.toolbar {
  display: flex;
  justify-content: end;
}
</style>
