<template>
    <div class="result">
    <h3 class="result">{{emitedResult}}</h3><h2>{{emitedChangeRate}}</h2>
    </div>
</template>

<script>
export default {
  name: 'Result',
  props: {
      emitedResult: Object, 
      emitedChangeRate: Object
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