<template>
  <div class="relative">
      <div class="nav-div flex justify-center  p-5">
          <img class="w-10 h-10 " src="~/static/img/logo.png" alt="">
          <h1 class="text-2xl text-center text-white ml-5">Weather Getter</h1>
      </div>
      <div class="content-div py-48">
          <div class="">
              <div class="container mx-auto px-5 py-10  right-10 bottom-15">
                  <span class="flex relative">


                      <input type="text" class="w-full p-5 rounded-full bg-gray-800 text-white text-sm md:text-xl"
                          placeholder="Search your Location" v-model="inputText">
                      <button @click="getWeather"
                          class="bg-green-500 py-4 px-10 font-bold text-lg rounded-full hover:bg-green-900  absolute top-1 right-0 ">
                          Search
                      </button>


                  </span>


              </div>

          </div>
          <!-- DISPLAY DIV -->
          <div v-show="display" class="container mx-auto  md:w-1/2 pb-20 ">
              <div class="rounded text-white">
                  <div class="p-3 flex justify-between bg-gray-700 font-bold">
                      <p class="uppercase">{{day}}</p>
                      <p class="uppercase">{{ monthYear}}</p>

                  </div>

                  <div class="bg-gray-800 py-3 px-20 pb-20 ">

                      <p class="text-3xl font-bold mb-10">{{city}}</p>

                      <div class="lg:flex  relative lg:mb-20">
                          <div class="relative flex">
                              <h1 class="text-2xl md:text-5xl font-extrabold">{{temp}}<sup>o</sup>C</h1>
                          </div>
                          <div class="lg:absolute -top-10 right-32">
                              <span class="flex"><img class="w-full" :src="getIcon" alt="weather-icon">{{descrip}}</span>

                          </div>

                      </div>


                      <div class="flex justify-between mt-10">
                          <div>
                              <img class="" src="~/static/img/icon-umberella.png" alt="">
                              <span>{{humidity}}%</span>
                          </div>
                          <div>

                              <span> <img class="" src="~/static/img/icon-wind.png" alt="">{{speed}} m/s</span>
                          </div>
                       

                      </div>

                  </div>


              </div>




          </div>
      </div>

  </div>

</template>

<script>
import Swal from 'sweetalert2'
export default {
  mounted: function () {
    
  },
  data() {
      return {
          display:false,
          results: [],
          inputText: '',
          gettext: '',
          city: '',
          getIcon: '',
          icon: '',
          descrip: '',
          temp: 0,
          humidity:'',
          speed:'',
          day: Date().slice(0,3),
      
          monthYear:Date().slice(3,10)
          
      }
  },
  methods: {
      getWeather() {
          var axios = require('axios');
          var data = '';

          var config = {
              method: 'get',
              url: 'https://api.openweathermap.org/data/2.5/weather?q=' + this.inputText + '&APPID=3a22e8a89ab65e992f817e2e457d1d48&units=metric',
              headers: {},
              data: data
          };

          axios(config)
              .then((response) => {

                  this.results = JSON.stringify((response.data));
                 
                  this.city = response.data['name'];
                  this.temp =   Math.trunc(response.data['main']['temp']);
               
                  this.descrip = response.data['weather'][0]['description'];

                  this.icon = response.data['weather'][0]['icon'];
                  
                  this.getIcon = `https://openweathermap.org/img/wn/${this.icon}@4x.png`;
                  this.humidity = response.data['main']['humidity'];
                  this.speed = response.data['wind']['speed'];

                  this.display=true;

                 

              })
              .catch(function (error) {
                  console.log(error);
                  Swal.fire('Invalid City')
              });


      },
      


  },

}
</script>

<style scoped>
.nav-div {
  background: #1e202b;
  color: #bfc1c8;
}

.content-div {
  background: linear-gradient(90deg, rgba(21, 18, 18, 0.9), rgba(57, 54, 54, 0.9)), url('~/static/img/city2.jpg');
  background-repeat: no-repeat;
  background-size: cover;
 

}

input {
  outline-offset: none;
  outline-color: aqua;
}
</style>