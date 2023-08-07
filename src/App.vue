<template>
  <weather
    :src="src"
  />
</template>

<script>
import Weather from "@/components/Weather";

export default {
  name: 'App',
  data(){
    return{
      src: 'https://api.openweathermap.org/data/2.5/weather?appid=261514ec6ead072a338a344dce0fb58f',
    }
  },
  components: {
    Weather
  }
}
</script>

<style>

</style>
