<template>
  <div class="min-h-screen h-screen max-h-screen flex flex-col">
    <header class="flex flex-col pt-2 md:pt-4 pb-12 md:pb-24 px-2 relative">
        <h1 class="font-mono text-3xl mb-2 md:mb-5 text-gray-200">IP Address Tracker</h1>
      <form @submit.prevent="getIpInfo" class="search flex max-w-xl w-full mx-auto font-mono ">
        <input v-model="queryIp" placeholder="Search any IP address or leave it blank to fetch current IP" type="text" class="w-full text-base md:text-2xl py-1 px-3 focus:outline-none rounded-tl-md rounded-bl-md" />
        <button type="submit" @click="getIpInfo" class="bg-black p-2 h-full rounded-tr-md rounded-br-md"><svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-gray-200" fill="none" viewBox="0 0 24 24" stroke="currentColor">
  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
</svg></button>
      </form>
      <Ipinfo v-if="ipData.value" :ipData="ipData.value" />
    </header>
    <div id="map" class="h-full z-10"></div>
  </div>
</template>

<script>
import Ipinfo from "./components/Ipinfo.vue";
import leaflet from "leaflet";
import { onMounted, ref, reactive } from "@vue/runtime-core";
import axios from 'axios'

export default {
  name: "App",
  components: {
    Ipinfo,
  },
  setup() {
    let map;
    const queryIp = ref('')
    const ipData = reactive({})
    
    onMounted(() => {
      map = leaflet.map("map").setView([51.505, -0.09], 9);
      
      leaflet
        .tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
          attribution:
            '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
        })
        .addTo(map);
    });
    
    const getIpInfo = async () => {
      try{
        const response = await axios(`https://geo.ipify.org/api/v1?apiKey=at_BtlDH0GZhLrj3qwiPgRyGlOyS0898&ipAddress=${queryIp.value}`)
        const data = response.data
       
        ipData.value = {
          ip: data.ip,
          isp: data.isp,
          timezone: data.location.timezone,
          location: data.location.region,
          lat: data.location.lat,
          lng: data.location.lng,
        }
        const {lat, lng} = ipData.value
       
       
        map.flyTo([lat, lng], 13, {duration:5});
        leaflet.marker([lat, lng]).addTo(map)
        
        

        setTimeout(() => {
          leaflet.marker([lat, lng]).addTo(map)
          .bindPopup(`${data.location.city}, ${data.location.country}`, {closeButton: false})
          .openPopup()
        }, 4500);

        
      }
      catch(err) {
        ipData.value = {}
        
      }
    }
    return {queryIp, ipData, getIpInfo, }
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
}
header {
  background-color: #3c40c6;

background-image: url("https://www.transparenttextures.com/patterns/inspiration-geometry.png");
 
}
.leaflet-popup-content-wrapper {
  margin-left: -10px;
}
.leaflet-popup-content {
 font-size: 1.2rem;
 margin: 0.5rem;
 white-space: nowrap;
 width: auto !important;
font-weight: 500;
}
.leaflet-marker-icon {
  filter: hue-rotate(30deg) saturate(100%) contrast(200%) !important
}
.leaflet-control-zoom {
  margin-top: 50px !important;
}
</style>
