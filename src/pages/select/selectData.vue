<template>
  <div class="select_data">
    <data-list v-for="item in testData" :key="item.id" :testData="item"/>
    <h2 v-if="dataNo">暂无该学员数据</h2>
  </div>
</template>

<script>
import dataList from '../../common/dataList'
export default {
  name: '',
  components:{dataList},
  props:{},
  data(){
    return {
      dataNo:false,//验证testData是否为空
      testData:[]
    }
  },
  mounted(){
    let _this = this
    uni.getStorage({
      key:'selectData',
      success:function(res){
        _this.testData = res.data
        _this.dataNo = _this.testData.length === 0 ? true : false
      }
    })
  }
}
</script>
<style scoped>
.select_data{
  background-color: #F1F5F9;
  padding-bottom: 10rpx;
}

.select_data > h2{
  background-color: white;
  padding-top: 45vh;
  text-align: center;
}
</style>