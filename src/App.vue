<template>
  <div id="app" >
   <div class="container">
      <div class="row">
        <div class="col-lg-2">
          <img src="./assets/logo.png" alt="La Clémentine" />
        </div>
        <div class="col-lg-10 d-flex flex-column justify-content-center">
          <h1>La Clémentine</h1>
          <p>Sandwicherie, petite restauration, plateau repas</p>
        </div>
      </div>
      <div class="row">
          <div id="days" class="col-lg-2" v-for="(day, indice) in days" :key="indice" style="overflow: hidden;">
              <Day :day="{day}.day" />
          </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import moment from 'moment'
import Day from './components/Day'

export default {
  name: 'App',
  components: { Day },
  data () {
    return {
      longitude: '',
      latitude: '',
      url: 'http://api.openweathermap.org/data/2.5/forecast?',
      api: 'ed471b8ac581aeaf7ac5f5be67bcdf11',
      lang: 'fr',
      unit: 'metric',
      city: 'Saint-Nicolas,be',
      days: [],
      length: 5
    }
  },
  mounted () {
    moment.locale('fr')
    this.getLocation()
    axios.get(this.send_uri)
      .then(response => {
        // handle success
        console.log(response.data.list)
        this.setDays(response.data.list)
      })
      .catch(function (error) {
        // handle error
        console.log(error)
      })
  },
  computed: {
    send_uri: function () {
      return this.url +
        '&APPID=' + this.api +
        '&q=' + this.city +
        '&units=' + this.unit +
        '&lang=' + this.lang
    }
  },
  methods: {
    getLocation: function () {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition((position) => {
          this.latitude = position.coords.latitude
          this.longitude = position.coords.longitude
        })
      }
    },
    setDays: function (data) {
      let d = {}
      let flag1 = false
      let flag2 = false
      for (var i = 0; i < 40; i++) {
        console.log(data[i])
        if (data[i].dt_txt.includes('09:00')) {
          console.log('*** ' + moment.unix(data[i].dt).format('dddd') + ' ***')
          d.day = moment.unix(data[i].dt).format('dddd')
          d.day_nr = moment.unix(data[i].dt).format('LL')
          d.temp_min = data[i].main.temp_min.toFixed(1)
          flag1 = true
        }
        console.log(data[i].dt_txt)
        if (data[i].dt_txt.includes('15:00')) {
          d.temp_max = data[i].main.temp_max.toFixed(1)
          d.description = data[i].weather[0].description
          d.icon = data[i].weather[0].icon
          flag2 = true
        }

        if (flag1 === true && flag2 === true) {
          this.days.push(d)
          d = {}
          flag1 = false
          flag2 = false
        }
        /* this.days.push(
          {
            day: moment.unix(data[i].dt).format('dddd'),
            day_nr: moment.unix(data[i].dt).format('LL'),
            temp_min: data[i].main.temp_min.toFixed(1),
            temp_max: data[i].main.temp_max.toFixed(1),
            descr: data[i].weather[0].description,
            description: data[i].weather[0].icon
          }) */
      }
    }
  }
}
</script>

<style>
#app {
  background-image: url('./assets/adam-excell-20323-unsplash.jpg');
  background-size: cover;
  background-repeat: no-repeat;
  background-position-y: -50vh;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  width: 100vw;
  height: 100vh;
  padding-left: 12%;
}

#days {
  background-color: rgba(255, 255, 255,.4);
}

.fade-enter-active,
.fade-leave-active{
    transition: opacity 5s
}
.fade-enter,
.fade-leave-to{
    opacity: 0
}
.fade-enter-active, .fade-leave-active {
  transition: opacity 5s;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>
