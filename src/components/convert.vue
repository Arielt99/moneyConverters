<template>
  <div class="convert">
    <ion-input v-on:keyup.enter="convert()" @input="setAmount($event.target.value)" value="" showCancelButton="always" id="amount" clear-icon cancel-button-text cancel-button-icon="md-arrow-back" placeholder="votre montant en euros..."></ion-input>
        <div class="error" v-if="!$v.amount.required">Field is required</div>
        <div class="error" v-if="!$v.amount.numeric">numeric is required</div>
        <ion-select
        placeholder="transformer en :"
        id="selectedOption"
      >
        <ion-select-option
          v-for="(currency, currencyCode) in list"
          :key="currencyCode"
          :value="currencyCode"
        >{{currency}}</ion-select-option>
      </ion-select>
      <ion-button color="primary" @click="convert()">
        chercher
      </ion-button>
      <p v-if="submitStatus === 'ERROR'">Please fill the form correctly.</p>
      <p v-if="submitStatus === 'PENDING'">Sending...</p>
  </div>
</template>

<script>
import { required, numeric } from 'vuelidate/lib/validators'
export default {
  name: 'Convertion',
  props: {},
  data (){
    return {
        apikey : process.env.VUE_APP_KEY,
        list:[],
        amount:"",
        result: null,
        changeRate:null,
        submitStatus: null
    }
},
validations: {
    amount: {
      required,
      numeric,
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
    convert(){
      this.$v.$touch()
      if (this.$v.$invalid) {
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
            this.emit(this.result,this.changeRate)
        })
        this.submitStatus = 'PENDING'
        setTimeout(() => {
          this.submitStatus = null
        }, 200)
      }
    },
    emit(result, changeRate){
      this.$emit('resultEmit',{result, changeRate})
    }
},
created: function(){
    this.currency_list()
}
}
</script>