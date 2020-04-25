<template>
  <div class="convert">
    <ion-item>
      <ion-label position="stacked">Amount :</ion-label>
      <ion-input v-on:keyup.enter="convert()" @input="setAmount($event.target.value)" value="" showCancelButton="always" id="amount" clear-icon cancel-button-text cancel-button-icon="md-arrow-back" placeholder="Your amount in euros..."></ion-input>
    </ion-item>
    <ion-item >
      <ion-label>To:</ion-label>
      <ion-select id="selectedOption" @ionChange="setSelectedOption($event.target.value)">
        <ion-select-option v-for="(currency, currencyCode) in list" :key="currencyCode" :value="currencyCode">{{currency}}</ion-select-option>
      </ion-select>
    </ion-item>
    <ion-button expand="block" color="primary" @click="convert()">
      Convert
    </ion-button>
    <div class="error">
      <p v-if="submitStatus === 'NUM'">numeric is required</p>
      <p v-else-if="submitStatus === 'ERROR'">Please fill the form correctly.</p>
      <p v-else-if="submitStatus === 'PENDING'">Sending...</p>
      <p v-else></p>
    </div>
  </div>
</template>

<script>
      //<div class="error" v-if="!$v.selectedOption.required">Field is required</div>
import { required, numeric } from 'vuelidate/lib/validators'
export default {
  name: 'Convertion',
  props: {},
  data (){
    return {
        apikey : process.env.VUE_APP_KEY,
        list:[],
        amount:"",
        selectedOption:"",
        result: null,
        changeRate:null,
        submitStatus: null
    }
},
validations: {
    amount: {
      required,
      numeric
    },
    selectedOption: {
      required
    }
  },
methods:{
    currency_list(){
      fetch('http://data.fixer.io/api/symbols?access_key='+this.apikey)
      .then(response => response.text())
      .then(data => {
          let parsedDataList = JSON.parse(data)
          let symbolsList = parsedDataList.symbols
          this.list = symbolsList
      })
    },
    setAmount(value) {
      this.amount = value
      this.$v.amount.$touch()
    },
    setSelectedOption(value) {
      this.selectedOption = value
      this.$v.selectedOption.$touch()
    },
    convert(){
      this.$v.$touch()
      if(this.$v.amount.numeric==false){
        this.submitStatus = 'NUM'
      }
      else if (this.$v.$invalid){
        this.submitStatus = 'ERROR'
      }
      else if (amount.value == ""){
        this.submitStatus = 'ERROR'
      }
      else {
        this.result=null
        this.changeRate = selectedOption.value
        fetch('http://data.fixer.io/api/latest?access_key='+this.apikey+'&symbols='+this.changeRate)
        .then(response => response.text())
        .then(data => {
            let parsedDataList = JSON.parse(data)
            let rates = parsedDataList.rates
            this.result = amount.value * rates[this.changeRate]
            this.result = this.result.toFixed(2)
            this.emit(this.result,this.changeRate, amount.value)
            amount.value=""
        })
        this.submitStatus = 'PENDING'
        setTimeout(() => {
          this.submitStatus = null
        }, 500)
      }
    },
    emit(result, changeRate, amount){
      this.$emit('resultEmit',{result, changeRate, amount})
    }
},
created: function(){
    this.currency_list()
}
}
</script>

<style>

ion-item {
  width: 94vw;
}
ion-button{
  width: 94vw;
}
ion-select{
  max-width: 70vw !important;
}
.error{
  height: 2vh;
}
</style>