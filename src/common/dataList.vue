<template>
  <div class="data-list">
    <div class="data-list-left">
      <p>{{year}}</p>
      <p>{{day}}</p>
    </div>
    <div class="data-list-right">
      <p class="name-card"><span>{{testData.name}}</span>{{testData.card}}</p>
      <p class="car"><span>{{testData.subject}}</span><span>{{testData.car}}</span>{{testData.agent}}</p>
      <p>{{testData.address}}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: '',
  components:{},
  props:{
    testData:{
      type:Object
    }
  },
  computed:{
    year(){
      return this.testData.date.substr(5)
    },
    day(){
      return this.testData.date.substr(0,4)
    }
  },
  data(){
    return {
    }
  },
}
</script>
<style scoped>
.data-list{
  display: flex;
  padding: 25rpx;
  text-align: center;
  background-color: white;
  margin-bottom:20rpx;
}

.data-list-left{
  flex-basis: 150rpx;
  padding: 24rpx;
  text-align: center;
}

.data-list-left p:nth-of-type(1){
  font-size: 45rpx;
}

.data-list-right{
  flex: 1;
  text-align: left;
}

.data-list-right p:nth-of-type(1){
  font-size: 34rpx;
}

.data-list-right p:nth-of-type(1) span{
  display: inline-block;
  width: 145rpx;
}

.data-list-right p:nth-of-type(2){
  font-size: 30rpx;
  color: #B6ADAD;
}

.data-list-right p:nth-of-type(2) span{
  display: inline-block;
  width: 150rpx;
}

.data-list-right p:nth-of-type(2),.data-list-right p:nth-of-type(3){
  margin-top: 10rpx;
}

.data-list-right p:nth-of-type(3){
  font-size: 26rpx;
}
</style>