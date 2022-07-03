<template>
  <div class="flex flex-col h-screen max-h-screen">
    <!-- Search Result -->
    <div class="z-20 flex justify-center relative bg-hero-pattern bg-cover px-4 pt-8 pb-32">
      <!-- Search Input -->
      <div class="w-full max-w-screen-sm">
        <h1 class="text-white text-center text-3xl pb-4">IP Addresss Tracker</h1>
        <div class="flex">
          <input v-model="queryIp" class="flex-1 py-3 px-2 rounded-tl-md rounded-bl-md focus:outline-none" type="text" placeholder="Search for any IP address or leave it empty to get your IP info" />
          <i @click="getIpInfo" class="cursor-pointer bg-black text-white px-4 rounded-tr-md rounded-br-md flex items-center fa-solid fa-chevron-right"></i>
        </div>
      </div>
      <!-- IP Info -->
      <IPInfo v-if="ipData" v-bind:ipData="ipData" />
    </div>

    <!-- Map -->
     <div id="map" class="h-full z-10"></div>
  </div>
</template>

<script>
// @ is an alias to /src
import IPInfo from '@/components/IPInfo.vue'
import leaflet from 'leaflet'
import {onMounted, ref} from 'vue'
import axios from 'axios'
import {GEOAPIFY_API_KEY} from '../../config.env'

export default {
  name: "HomeView",
  components: {IPInfo},
  setup() {
    let mymap
    const queryIp = ref("")
    const ipData = ref(null)

    const getIpInfo = async () => {
      // geo.ipify.org
      // const url = queryIp?.value === "" 
      //   ? `https://geo.ipify.org/api/v2/country,city?apiKey=${API_KEY}` 
      //   : `https://geo.ipify.org/api/v2/country,city?apiKey=${API_KEY}&ipAddress=${queryIp?.value}`
      
      // geoapify.com
      const url = queryIp?.value === "" 
        ? `https://api.geoapify.com/v1/ipinfo?apiKey=${GEOAPIFY_API_KEY}` 
        : `https://api.geoapify.com/v1/ipinfo?apiKey=${GEOAPIFY_API_KEY}&ip=${queryIp?.value}`
      
      try {
        const response = await axios.get(url)
        const result = response?.data
        console.log(result)
        // // geoipfy
        // ipData.value = {
        //   ip: result?.ip,
        //   address: result?.location,
        //   proxy: result?.proxy,
        // }
        // // set marker
        // leaflet.marker([ipData?.value?.address?.lat, ipData?.value?.address?.lng]).addTo(mymap);
        // // center map to marker
        // mymap.setView([ipData?.value?.address?.lat, ipData?.value?.address?.lng], 14);

        // geoapify
        ipData.value = {
          ip: result?.ip,
          country: result?.country?.name,
          state: result?.state?.name,
          city: result?.city?.name,
          latitude: result?.location?.latitude,
          longitude: result?.location?.longitude,
        }
        // set marker
        leaflet.marker([ipData?.value?.latitude, ipData?.value?.longitude]).addTo(mymap);
        // center map to marker
        mymap.setView([ipData?.value?.latitude, ipData?.value?.longitude], 14);
      } catch (error) {
        console.log(error?.message)
      }
    }

    onMounted(() => {
      mymap = leaflet.map('map').setView([51.505, -0.09], 9);

      leaflet.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        maxZoom: 19,
        attribution: '© OpenStreetMap'
      }).addTo(mymap);

      // auto detect user location on load
      getIpInfo()
    })

    return { queryIp, ipData, getIpInfo }
  }
};
</script>
