<template>
  <div class="upload">
    <view class="tui-box-upload">
      <p>请按顺序选择要上传的身份证正面(头像面)、身份证反面(国徽面)，半身照共三张图片</p>
			<tui-upload ref="picClear" :serverUrl="serverUrl" @alertShow="alertShow" @baidu="baidu" @complete="result" @remove="remove" id="upload"></tui-upload>
      <view class="form-text">
        <view class="title">学员姓名：</view>
         <input class="uni-input" v-model="form.name" confirm-type="done" type="text" placeholder="学员姓名" />
      </view>
      <view class="form-text">
        <view class="title">身份证：</view>
         <input class="uni-input" v-model="form.card" confirm-type="done" type="text" placeholder="身份证" />
      </view>
      <view class="form-text">
        <view class="title">身份证地址：</view>
         <input class="uni-input" v-model="form.address" confirm-type="done" type="text" placeholder="身份证地址" />
      </view>
      <picker @change="PickerChange" :range="array" id="picker">
        <label style="width:200rpx">车型：</label>
        <label>{{array[index]}}</label>
      </picker>
      <view class="form-text">
        <view class="title">加盟商：</view>
        <input class="uni-input" v-model="form.agent" confirm-type="done" type="text" placeholder="加盟商号" />
      </view>
      <view class="form-text">
        <view class="title">学员电话：</view>
        <input class="uni-input" v-model="form.phone" confirm-type="done" type="text" placeholder="学员电话" />
      </view>
      <view class="form-text">
        <view class="title">备注：</view>
        <input class="uni-input" v-model="form.ps" confirm-type="done" type="text" placeholder="备注" />
      </view>
		</view>
    <div class="footer-bottom">
      <button type="default" @click="back()">返回</button>
      <button type="primary" @click="dataPost()">提交</button>
    </div>
    <tui-alert 
      :show="show" 
      @click="hideAlert"
      @cancel="hideAlert"
      :maskClosable="maskClosable" 
      :btnColor="btnColor"
		  :color="color" 
      :btnText="btnText"
			class="upload_alert">
			{{alertText}}
		</tui-alert>
  </div>
</template>

<script>
export default {
  name: '',
  props:{},
  data(){
    return {
      imageData: [],
				//上传地址
      serverUrl: "http://192.168.2.116:8888/NotPayingPhoto",
      array:['C1','C2','C3','B1','B2','A1','A2','D1','D2','C1E','C2E'],
      index:0,
      form:{
        name:'',
        card:'',
        address:'',
        car:'C1',
        agent:'',
        phone:'',
        ps:'',
      },
      value:'',
      schoolName:'',
      show:false,
      maskClosable: false,
      btnColor: "#EB0909",
      color: "#333",
      btnText: "确定",
      alertText:'ok'
    }
  },
  methods:{
    result(e) {
      console.log(e)
      this.imageData = e.imgArr;
    },
    remove(e) {
      //移除图片
      console.log(e)
      let index = e.index
    },
    PickerChange(e){
      this.index = e.target.value
      this.form.car = this.array[this.index]	
    },
    back(){
      uni.navigateBack({
      delta: 1
      })
    },
    baidu(val){
      for (let key in val){
        switch(key){
          case '姓名':
            this.form.name = val[key].words
            break;
          case '住址':
            this.form.address = val[key].words
            break;
          case '公民身份号码':
            this.form.card = val[key].words
            break;
        }
      }
    },
    dataPost(){
      //取得form所有value
      console.log(this.form)
      let valueArr = Object.values(this.form)
      //删除最后为ps的数据，因为备注也可以为空
      console.log(valueArr)
      valueArr.pop()
      //身份证正则
      const cardReg = /^\d{6}(18|19|20)?\d{2}(0[1-9]|1[0-2])(([0-2][1-9])|10|20|30|31)\d{3}(\d|X|x)$/
      let card = cardReg.test(this.form.card)
      //电话正则
      const phoneReg = /^1[3|4|5|7|8][0-9]\d{8}$/
      let phone = phoneReg.test(this.form.phone)
      console.log(valueArr)
      if(valueArr.includes('')){
        this.alertShow('除了备注外，选项都不能为空')
      }else if(!card){
        this.alertShow('身份证号码格式错误，请重新验证')
      }else if(!phone){
        this.alertShow('手机号码格式错误，请重新验证')
      }else{
        let formData = Object.assign({}, this.form)
        formData['school'] = this.schoolName
        formData['file'] = this.imageData
        uni.request({
          url:'http://192.168.2.116:8888/NotPayingInsert',
          method:'POST',
          data:formData,
          success:(res)=>{
            console.log(res);
            switch(res.data.status){
              case 'onpay':
                this.alertShow('该学员已付款')
                break
              case 'oncard':
                this.alertShow('该学员已报名')
                break
              case 'agent':
                this.alertShow('该加盟商不存在')
                break
              case 'ok':
                this.alertShow('报名成功')
                break
            }
          }
        })
      }
    },
    hideAlert(type) {
      this.show = false
      if(this.alertText == '报名成功'){
        for(let key in this.form){
        key !== 'car'? this.form[key] = '' : ''
      }
        this.imageData = [],
        this.$refs.picClear.imageList = [],
        this.$refs.picClear.statusArr = []
      }
    },
    alertShow(val){
      this.show = true
      this.alertText = val
    }
  },
  mounted() {
    let _this = this
    uni.getStorage({
      key:'school',
      success:function(res){
        _this.schoolName = res.data
      }
    })
  },
  destroyed(){
    let imglist = []
    this.imageData.map(item => {
      imglist.push(item)
    })
    if(imglist.length > 0){
      uni.request({
        url:'http://192.168.2.116:8888/NotPayingPhotoDelete',
        method:'POST',
        data:{
          file:imglist
        },
        success:(res)=>{
        }
      })
    }
  }
}
</script>
<style>
.upload{
  background-color: #F1F5F9;
  height:100%;
  overflow: auto;
  overflow-x: hidden;
}

.tui-box-upload{
 height:calc(100vh - 46px)
}

.tui-box-upload > p,.tui-box-upload > view,#picker,.tui-container > .data-v-31bf054a{
  padding: 35rpx 40rpx;
  background-color: white;
  margin-bottom: 15rpx;
}

.tui-box-upload p{
  font-size: 30rpx;
}

.tui-upload-add.data-v-31bf054a,.tui-upload-box > .tui-image-item{
  width: 150rpx!important;
  height: 150rpx!important;
}

.tui-image-item > .tui-item-img{
  width: 100%!important;
  height: 100%!important;
}

.form-text{
  display: flex;
}

.form-text .title{
  flex-basis: 200rpx;
}

#picker label:nth-of-type(1){
  display: inline-block;
  width: 200rpx;
}

.footer-bottom{
  display: flex;
  position: fixed;
  bottom: 0;
  width: 100%;
  padding: 0!important;
  margin: 0!important;
  z-index: 999;
}

.footer-bottom button{
  border-radius: 0%;
  flex: 1;
}
.footer-bottom button::after{ border: none;}

.upload_alert{
  padding: 0;
  background-color:none;
  margin-bottom:0;
}
</style>