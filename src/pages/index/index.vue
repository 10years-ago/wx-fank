<template>
	<view class="content">
		<img src="@/static/fank2.jpg">
		<!-- <p>{{schoolName}}驾校</p>
		<button @click="upload()">上传资料</button> -->
		<main>
			<h2>驾校服务</h2>
			<ul>
				<li @click="upload()">
					<img src="@/static/userUpload.png">
					<p>上传资料</p>
				</li>
				<li @click="testSearch()">
					<img src="@/static/testSearch.png">
					<p>学员考试查询</p>
				</li>
				<li>3</li>
				<li>4</li>
				<li>5</li>
				<li>6</li>
				<li>7</li>
				<li>8</li>
				<li>9</li>
			</ul>
		</main>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'Hello',
				schoolName:''
			}
		},
		methods: {
			upload(){
				uni.navigateTo({
					url: '/pages/upload/upload'
				})
			},
			testSearch(){
				console.log(321)
				uni.navigateTo({
					url: '/pages/select/select'
				})
			}
		},
		mounted() {
			let _this = this
			uni.getStorage({
				key:'school',
				success:function(res){
					_this.schoolName = res.data
					uni.setNavigationBarTitle({
						title:res.data + '驾校'
					})
				}
			})
		}
	}
</script>

<style>
.content{
	background-color: #EDEDED;
	height: calc(100vh);
	text-align: center;
}

button{
  height:80rpx;
  margin-top: 30rpx;
	line-height: 80rpx;
	width: 400rpx;
}

.content > img{
	margin-top: 30rpx;
	width: 95%;
	height: 25%;
	border-radius: 1.5rem;
}

main{
	background-color: white;
	margin: 0 auto;
	margin-top: 30rpx;
	border-radius: 1rem;
	width: 95%;
}

main h2{
	border-bottom: 1px solid #EFEFEF;
	height: 100rpx;
	text-align: left;
	line-height: 100rpx;
	padding-left: 1em;
	box-sizing: border-box;
}

main ul{
	display: flex;
	flex-wrap: wrap;
	font-size: 22rpx;
}

main li{
	list-style: none;
	text-align: center;
	height: 200rpx;
	flex-basis: 33.333%;
	border: 1px solid #F1F1F1;
	box-sizing: border-box;
}

main li img{
	margin-top: 30rpx;
	width: 100rpx;
	height: 100rpx;
	margin-left: 15rpx;
}

</style>
