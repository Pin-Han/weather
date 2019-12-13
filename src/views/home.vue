<template>
  <div>
    <div class="carousel">
      <div class="carousel__img"></div>
      <div class="carousel__search">
        <select name="" id="" class="carousel__search-bar">
          <option value="">請選擇城市</option>
          <option value="">新北市</option>
        </select>
        <button class="btn" id="search">
          <i class="fas fa-search"></i>
        </button>
        <!-- <i class="fas fa-search carousel__search-icon"></i> -->
      </div>
      <div class="carousel__title">
        <i class="fas fa-cloud-sun"></i>
        Taiwan weather
        <!-- <i class="el-icon-cloudy-and-sunny"></i>
        <i class="far fa-sun"></i> -->
      </div>
    </div>
    <weather id="weather"></weather>
  </div>
</template>
<script>
import axios from "axios";
import $ from "jquery";
import weather from '@/views/weather.vue';

export default {
  data() {
    return {
      city:[]
    };
  },
  components:{
    weather,
  },
  methods: {
    getweather() {
      const vm =this;
      console.log(process.env.VUE_APP_WEATHER);
      function promise() {
        return new Promise(function(resolve, reject) {
          axios
            .get(process.env.VUE_APP_WEATHER)
            .then(res => {
              resolve(res);
            })
            .catch(error => {
              console.log(error);
            });
        });
      }
      promise().then(function(data) {
         vm.city = data.data.records.locations[0].location;
         console.log(vm.city);
      });
    }
  },
  mounted() {
    this.getweather();
    $("#search").click(function() {
      console.log(this);

      $("html,body").animate(
        {
          scrollTop: $("#weather").offset().top
        },
        1000
      );
    });
  }
};
</script>
