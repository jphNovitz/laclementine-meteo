<template>
<transition name="fade">
  <div id="day">
      <p>
      <span class="h1">  {{day.day}}</span> <br />
      <span>{{day.day_nr}}</span>
      </p>
      <p class="h3">
        Min:
        {{day.temp_min}}
      </p>
      <p>
        <img :src="'http://openweathermap.org/img/w/' + day.icon + '.png'" />
      </p>
      <p class="h3">
        Max:
        {{day.temp_max}}
      </p>
      <p class="h4">
        {{day.description}}
      </p>
  </div>
</transition>
</template>

<script>

export default {
  name: 'Day',
  components: {},
  props: ['day'],
  data () {
    return {
    }
  }
}
</script>

<style scoped>
img {
  width: 7rem;
}
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity ease 1.8s, transform ease-in-out .8s ;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  transform: translateX(1000px);
  opacity: 0;
}
#day{
}
</style>
