<template>
  <div class="app">
    <h1>Weather forecast app</h1>

    <form @submit.prevent="load()">
        <input type="search" placeholder="Enter region name. Ex Paris" v-model="location">
        <button :disabled='!location || loading'>Search</button>
    </form>

    <div v-if="loading" class="">Chargement ... </div>
    <div class="error-container" v-if="!data?.forecast?.length && !loading">{{ 'Aucune information trouvée pour cette région' }}</div>
    <div v-if="data?.forecast?.length">
        <!-- {{ data?.forecast }} -->

        <div class="main-card">
            <div class="current">
                <div>
                    <p class="location">{{ data.location.name }}, {{ data.location.country }}</p>
                    <i class="time">{{ dateForHumans(data.location.localtime) }}</i>
                </div>
                <div>
                    <div class="temp">
                        <p>
                            <img :src="data?.current?.condition?.icon" width="50" height="50" alt="img">
                            <span>{{  data?.current?.condition?.text }}</span>
                        </p>
                        <p>~{{ data.current.temp_c }}°</p>
                    </div>
                </div>
            </div>
            <div class="forecats">
                <div class="item" v-for="item in data?.forecast" :key="item.date">
                    <div class="content">
                        <span class="day">{{ dayNameFromDate(item.date) }}</span>
                        <img :src="item?.day?.condition?.icon" alt="icon" width="50" height="50">
                        <span class="temp">
                            ~{{ item?.day?.avgtemp_c }}°
                        </span>
                    </div>
                </div>
            </div>
        </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import moment from 'moment';
// a47a14d3c35a4b49a22125917211802
// http://api.weatherapi.com/v1/forecast.json?key=a47a14d3c35a4b49a22125917211802&q=Paris&days=7

export default {
  name: "Weather",
  props: {
    msg: String,
  },
  data: () => ({
      loading: true,
      location: 'Paris',
      apiKey: 'a47a14d3c35a4b49a22125917211802',
      forcastDaysCount: 10,
      data: {
          current: undefined,
          location: undefined,
          forecast: [],
      },
  }),
  methods: {
      async load() {
        this.loading = true;
        this.data.location = undefined;
        this.data.current = undefined;
        this.data.forecast = [];
        const url = `http://api.weatherapi.com/v1/forecast.json?key=${this.apiKey}&q=${this.location}&days=${this.forcastDaysCount}`;
        try {
            const res = await axios.get(url);
            const data = res?.data;
            const {location, forecast, current} = data;
            this.data.location = location ?? {};
            this.data.current = current ?? {};
            this.data.forecast = forecast?.forecastday || [];
            this.loading = false;
        } catch (e) {
            this.loading = false;
        }


      },
      dateForHumans(date) {
          return moment(date).format('LL');
      },
      dayNameFromDate(date) {
          return moment(date).format('ddd');
      },
  },
  mounted() {
    this.load();
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    h1 {
        text-transform: uppercase;
    }
    .main-card {
        width: 500px;
        height: 300px;
        border-radius: 5px;
        margin: auto;
        display: grid;
        grid-template-rows: 40% 60%;
    }

    .main-card .current {
        background-color: aqua;
    }
    .main-card .current .location {
        font-size: 1.2rem;
        font-weight: bold;
        padding: 0;
        margin-bottom: 0;
    }
    .main-card .current .time {
        font-size: 0.8rem;
    }

    .main-card .current .temp {
        display: flex;
        align-items: center;
        justify-content: space-between;
        padding: 0 2rem;
        font-weight: bold;
        font-size: 1.2rem;
    }

    .main-card .current .temp p {
        margin: 0;
    }

    .main-card .current .temp p img {
        vertical-align: middle;
    }


    .main-card .forecats {
        background-color: rebeccapurple;
        color: white;
        display: flex;
        justify-content: space-evenly;
        margin-top: 0.1rem;
        padding-top: 1rem;
    }

    .main-card .forecats span {
        font-weight: bold;
    }
    .main-card .forecats .item .content{
        display: flex;
        flex-direction: column;
        height: 100%;
        justify-content: space-around;
    }

    form {
        margin-bottom: 2rem;
    }
    input {
        width: 20vw;
        height: 2rem;
    }
    button {
        height: 2rem;
        margin-left: 1rem
    };
</style>