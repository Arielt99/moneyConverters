<template>
  <div id="app">
    <ion-app>
      <ion-vue-router />
      <Convertion v-on:resultEmit="resultEmit"/>
      <Result v-bind:emitedResult="emitedResult" v-bind:emitedChangeRate="emitedChangeRate" v-if="emitedResult"/>
    </ion-app>
  </div>
</template>
<script>
import Convertion from './components/convert'
import Result from './components/result'
export default {
  name: "home",
  components:{
    Convertion,
    Result
  },
  data (){
    return {
      emitedResult:null,
      emitedChangeRate:null
    }
  },
  methods: {
    resultEmit(payload){
      let {result, changeRate} = payload;
      this.emitedResult= result
      this.emitedChangeRate= changeRate
    }
  } 
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
</style>
