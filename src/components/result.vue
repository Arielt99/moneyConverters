<template>
    <div class="result">
        <ion-card>
            <p>{{originalAmount}} EUR</p>
            <ion-icon name="swap"></ion-icon>
            <h3 class="result">{{emitedResult}} {{emitedChangeRate}}</h3>
        </ion-card>
    </div>
</template>

<script>
export default {
  name: 'Result',
  props: {
      emitedResult: Object, 
      emitedChangeRate: Object,
      originalAmount: Object
      },
  data (){
    return {
    }
},
methods:{
},
created: function(){
}
}
</script>
<style>
.result{
    color:rgb(39, 39, 39);
}
ion-card{
    margin-top: 5vh;
}
</style>