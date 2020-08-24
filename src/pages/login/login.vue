<template>
  <div class="login">
    <div class="from">
      <label for="user">用户名</label>
			<input type="text" id="user" v-model="user" placeholder="请输入用户名" />
      <label for="password">密码</label>
			<input type="password" id="password" v-model="password" placeholder="请输入密码" />
      <button @click="login()">登陆</button>
		</div>
      <tui-alert 
      :show="show" 
      @click="hideAlert"
      @cancel="hideAlert" 
      :maskClosable="maskClosable" 
      :btnColor="btnColor"
		  :color="color" 
      :btnText="btnText">
			账号或密码错误
		</tui-alert>
  </div>
</template>

<script>
import { log } from 'util'
export default {
  data(){
    return {
      user: '',
      password:'',
      show: false,
      maskClosable: false,
      btnColor: "#EB0909",
      color: "#333",
      btnText: "确定"
    }
  },
  methods:{
    login(){
      uni.request({
        url:'http://192.168.2.116:8888/loginto',
        method:'POST',
        data:{
          user:this.user,
          password:this.password
        },
        success:(res)=>{
          console.log(res);
          if(res.data.status == 'ok' && res.data.perm == 'c'){
            uni.setStorage({key:'school',data:res.data.school})
            uni.setStorage({key:'baiduToken',data:{apiKey:res.data.apiKey,secretKey:res.data.secretKey}})
            uni.setStorage({key:'perm',data:res.data.perm})
            uni.navigateTo({
            url:'/pages/index/index'
          })
          }else{
            this.show = true
          }
        }
      })
    },
    hideAlert(type) {
				this.show = false
			}
  }
}
</script>
<style scoped>
.login{
  background-color: #408AE7;
  height: calc(100vh);
}

.from{
  width: 500rpx;
  margin: 0 auto;
  padding-top: 45%;
}

label{
  display: block;
  padding: 20rpx 0 20rpx 0;
  text-align: center;
}

input{
  color: black;
  padding: 15rpx;
  border-radius: 10rpx;
  background-color: white;
}

.from button{
  height:80rpx;
  margin-top: 30rpx;
  line-height: 80rpx;
}
</style>