<template>
  <div id="app">
    <ion-app>
      <ion-header>
        <ion-toolbar>
          <ion-title>Money Converter</ion-title>
        </ion-toolbar>
      </ion-header>
      <ion-content>
        <Convertion align="center" v-on:resultEmit="resultEmit"/>
        <Result align="center" v-bind:emitedResult="emitedResult" v-bind:emitedChangeRate="emitedChangeRate" v-bind:originalAmount="originalAmount" v-if="emitedResult"/>
      </ion-content>
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
      emitedChangeRate:null,
      originalAmount:null
    }
  },
  methods: {
    resultEmit(payload){
      let {result, changeRate, amount} = payload;
      this.emitedResult= result
      this.emitedChangeRate= changeRate
      this.originalAmount= amount
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
