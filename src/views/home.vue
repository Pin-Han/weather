<template>
  <div>
    <div class="carousel">
      <twmap class="carousel__map"></twmap>
      <div class="carousel__img"></div>
      <div class="carousel__search">
        <select name="" id="" class="carousel__search-bar" v-model="chooseCity">
          <option value="">請選擇城市</option>
          <option :value="item" v-for="item in city" :key="item">{{
            item
          }}</option>
        </select>
        <button class="btn" id="search" @click="search()">
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
    <div class="weather" id="weather" v-if="chooseCity != ''">
      <!-- <video autoplay muted loop class="weather__video">
        <source :src="video_url" type="video/mp4" />
        Sorry, your browser doesn't support embedded videos.
      </video> -->
      <img class="weather__img" :src="img_url" alt="tw_weather" />
      <div class="info">
        <div class="info__script">
          <div class="info__sort">
            <div class="info__sort-city">
              {{ chooseCity }}
            </div>
            <div class="info__sort-degree">{{ result.average }}°</div>
          </div>
          <div class="info__script-description">
            今天天氣，{{ result.descript }}
          </div>
        </div>
        <div class="info__week">
          <i class="fas fa-cloud-sun"></i>
          <i class="fas fa-cloud-sun"></i>
          <i class="fas fa-cloud-sun"></i>
          <i class="fas fa-cloud-sun"></i>
          <i class="fas fa-cloud-sun"></i>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";
import $ from "jquery";
import twmap from "@/components/map.vue";

export default {
  components: { twmap },
  data() {
    return {
      city: [
        "台北市",
        "基隆市",
        "新北市",
        "連江縣",
        "宜蘭縣",
        "新竹市",
        "新竹縣",
        "桃園市",
        "苗栗縣",
        "台中市",
        "彰化縣",
        "南投縣",
        "嘉義市",
        "嘉義縣",
        "雲林縣",
        "台南市",
        "高雄市",
        "澎湖縣",
        "金門縣",
        "屏東縣",
        "台東縣",
        "花蓮縣"
      ],
      chooseCity: "",
      result: {
        average: "", //平均溫度
        chance: "", //降雨機率
        descript: "" //天氣描述
      },
      date: "",
      phenomenon: "", //天氣現象
      img_url: "",
      weatherUrl: {
        rain:
          "https://images.unsplash.com/photo-1519692933481-e162a57d6721?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1500&q=80",
        cloud:
          "https://images.unsplash.com/photo-1501630834273-4b5604d2ee31?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80",
        cloudsDay:
          "https://images.unsplash.com/photo-1469765869284-c3821b02cf48?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80",
        wind:
          "https://images.unsplash.com/photo-1456356627738-3a96db6e2e33?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1358&q=80",
        sun:
          "https://images.unsplash.com/photo-1525490829609-d166ddb58678?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1349&q=80"
      }
    };
  },
  methods: {
    getweather(data) {
      //取得中央氣象局資料
      const vm = this;
      function promise() {
        return new Promise(function(resolve, reject) {
          axios
            .get(`${process.env.VUE_APP_WEATHER}&locationName=${data}`)
            .then(res => {
              resolve(res);
            })
            .catch(error => {
              reject(error);
            });
        });
      }
      promise().then(function(data) {
        const nodata = data.data.records.locations[0].location.length;
        const road = data.data.records.locations[0].location[0].weatherElement;
        if (nodata === 0) {
          alert("很抱歉");
          return;
        }
        //台北的天氣有問題
        vm.result.chance = road[0].time[0].elementValue[0].value;
        vm.result.average = road[1].time[0].elementValue[0].value;
        vm.result.descript = road[10].time[0].elementValue[0].value;
        vm.phenomenon = road[6].time[0].elementValue[0].value;
        const today = new Date();
        vm.date = `${today.getMonth()}/${today.getDate()}`;
        vm.checkImg(vm.phenomenon);
        vm.loading = false;
      });
    },
    search() {
      //搜尋縣市
      this.loading = true;
      this.getweather(this.chooseCity);
    },
    checkImg(data) {
      //判斷天氣，顯示不同的圖片
      const vm = this;
      const weatherImg = {
        陰短暫雨: () => {
          vm.img_url = vm.weatherUrl.rain;
        },
        多雲: () => {
          vm.img_url = vm.weatherUrl.cloud;
        },
        多雲時晴: () => {
          vm.img_url = vm.weatherUrl.cloud;
        },
        陰天: () => {
          vm.img_url = vm.weatherUrl.cloudsDay;
        },
        晴時多雲: () => {
          vm.img_url = vm.weatherUrl.cloud;
        },
        多雲短暫雨: () => {
          vm.img_url = vm.weatherUrl.rain;
        }
      };
      weatherImg[data]();
    }
  },
  mounted() {
    $("#search").click(function() {
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
