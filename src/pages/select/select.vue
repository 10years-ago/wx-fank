<template>
  <div class="select">
    <view class="form-text">
      <view class="title">学员姓名：</view>
      <input class="uni-input" v-model="name" confirm-type="done" type="text" placeholder="学员姓名" />
    </view>
    <view class="form-text">
      <view class="title">身份证：</view>
      <input class="uni-input" v-model="card" confirm-type="done" type="text" placeholder="身份证" />
    </view>
    <view class="form-text">
      <view class="title">加盟点：</view>
      <input class="uni-input" v-model="agent" confirm-type="done" type="text" placeholder="加盟点" />
    </view>
    <view class="uni-list-cell">
      <view class="uni-list-cell-left">
            选择日期
        </view>
        <view class="uni-list-cell-db">
            <picker mode="date" :value="date" :start="startDate" :end="endDate" @change="bindDateChange">
                <view class="uni-input">{{date}}</view>
            </picker>
        </view>
        <icon type="clear" size="26" @click.stop="dateclear()"/>
    </view>
    <button type="primary" @click="select()">查询</button>
  </div>
</template>

<script>
export default {
  components:{},
  props:{},
  data(){
    return {
      date: '全部时间',
      name:'',
      card:'',
      agent:'',
      school:''
    }
  },
  computed: {
    startDate() {
        return this.getDate('start');
    },
    endDate() {
        return this.getDate('end');
    }
  },
   methods: {
    bindDateChange(e) {
        this.date = e.target.value
    },
    getDate(type) {
        const date = new Date();
        let year = date.getFullYear();
        let month = date.getMonth() + 1;
        let day = date.getDate();

        if (type === 'start') {
            year = year - 60;
        } else if (type === 'end') {
            year = year + 2;
        }
        month = month > 9 ? month : '0' + month;;
        day = day > 9 ? day : '0' + day;
        return `${year}-${month}-${day}`;
    },
    dateclear(){
      this.date = '全部时间'
    },
    select(){
      let allDate = this.date == '全部时间' ? '': this.date
      //这个是接口要发送的数据，别问我为什么要传空，后端需求，接口和软件上接口一样。
      let selectData = {
        name:this.name,
        studyCard:'',
        card:this.card,
        subject:'',
        car:'',
        agent:this.agent,
        date:allDate,
        end_date:allDate,
        address:'',
        phone:'',
        agent:'',
        school:this.school,
        page:1,
        pageSize:200
      }
      uni.request({
        url:'http://192.168.2.116:8888/testStuAll',
        method:'POST',
        data:selectData,
        success:(res)=>{
          uni.setStorage({
            key: 'selectData',
            data: res.data.list,
          });
          uni.navigateTo({
            url: '/pages/select/selectData'
          })
        }
      })
    }
  },
  mounted(){
    let _this = this
    uni.getStorage({
      key:'school',
      success:function(res){
        _this.school = res.data
      }
    })
  }
}
</script>
<style scoped>
.select{
  background-color: #F1F5F9;
  height: 100vh;
  padding-top: 50rpx;
}

.uni-list-cell,.form-text{
  display: flex;
  padding: 25rpx 40rpx;
  margin-top: 15rpx;
  margin-bottom: 15rpx;
  background-color: white;
}

.uni-list-cell-left,.form-text .title{
  flex-basis: 200rpx;
}

.uni-list-cell-db{
  flex:1
}

button{
  margin-top: 20%;
  width: 80%;
}
</style>