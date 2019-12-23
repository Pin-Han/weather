<template>
  <div>
    <div class="carousel">
      <twmap class="carousel__map" @chooseCity="getweather"></twmap>
      <div class="carousel__search">
        <el-select
          v-model="chooseCity"
          placeholder="請選擇縣市"
          @change="getweather(chooseCity)"
        >
          <el-option :value="item" v-for="item in city" :key="item"
            >{{ item }}
          </el-option>
        </el-select>
      </div>
      <div class="carousel__result">
        <div class="carousel__city">
          <p class="carousel__city-city">
            {{ chooseCity }}
          </p>
          <p class="carousel__city-degree">
            {{ result.average }}
          </p>
        </div>

        <p class="carousel__result-descript">
          {{ result.descript }}
        </p>
        <p class="carousel__result-descript" v-if="!apiData">
          很抱歉，暫無{{ chooseCity }}天氣資訊，請選取其他縣市取得天氣資訊。
        </p>
      </div>
      <div class="carousel__title">
        <i class="fas fa-cloud-sun"></i>
        Taiwan weather
      </div>
    </div>
  </div>
</template>
<script>
import axios from 'axios';
import twmap from '@/components/map.vue';

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
      apiData: false, // api資料是否有
    };
  },
  methods: {
    getweather(data) {
      //取得中央氣象局資料
      const vm = this;
      vm.chooseCity = data;
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
        const nodata = data.data.records.locations[0].location;
        if (nodata.length === 0) {
          vm.apiData = false;
          vm.result = [];
          return;
        } else {
          vm.apiData = true;
          const road =
            data.data.records.locations[0].location[0].weatherElement;
          //台北的天氣有問題
          vm.result.chance = road[0].time[0].elementValue[0].value;
          vm.result.average = `${road[1].time[0].elementValue[0].value}°`;
          vm.result.descript = `今日天氣${road[10].time[0].elementValue[0].value}`;
        }
      });
    },
    search() {
      //搜尋縣市
      this.getweather(this.chooseCity);
    },
  },
};
</script>
