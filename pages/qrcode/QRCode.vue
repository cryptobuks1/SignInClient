<template xlang="wxml">
	<view>
		<view class="title">
			<text>二维码签到</text>
		</view>
		<view class="qrimg">
			<view class="qrimg-i">
				<tki-qrcode v-if="ifShow" cid="qrcode2" ref="qrcode2" val="val" :size="size" :onval="onval" :loadMake="loadMake" :usingComponents="true" @result="qrR" />
			</view>
		</view>
		<view>
			<text>数据</text>
			<text>{{val}}</text>
		</view>
	</view>
</template>
<script>
import tkiQrcode from '@/components/tki-qrcode/tki-qrcode.vue'
export default {
	data() {
		return {
			ifShow: true,
			val: this.indentityData, // 要生成的二维码值
			size: 300, // 二维码大小
			unit: 'upx', // 单位
			background: '#b4e9e2', // 背景色
			foreground: '#309286', // 前景色
			pdground: '#32dbc6', // 角标色
			icon: '', // 二维码图标
			iconsize: 40, // 二维码图标大小
			lv: 3, // 二维码容错级别 ， 一般不用设置，默认就行
			onval: true, // val值变化时自动重新生成二维码
			loadMake: true, // 组件加载完成后自动生成二维码
			src: '' ,// 二维码生成后的图片地址或base64
			indentityData: '',
		}
	},
	methods: {
		sliderchange(e) {
			this.size = e.detail.value
		},
		creatQrcode() {
			this.$refs.qrcode._makeCode()
		},
		saveQrcode() {
			this.$refs.qrcode._saveCode()
		},
		qrR(res) {
			this.src = res
		},
		clearQrcode() {
			this.$refs.qrcode._clearCode()
			this.val = ''
		},
		ifQrcode() {
			this.ifShow = !this.ifShow
		},
		selectIcon() {
			let that = this
			uni.chooseImage({
				count: 1, //默认9
				sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
				sourceType: ['album'], //从相册选择
				success: function (res) {
					that.icon = res.tempFilePaths[0]
					setTimeout(() => {
						that.creatQrcode()
					}, 100);
					// console.log(res.tempFilePaths);
				}
			});
		},
	},
	components: {
		tkiQrcode
	},
	onLoad: function () { 
		uni.getStorage({
			key:'a1',
			success: function(res) {
				this.indentityData = res.data
				
			}
		})
	},
}
</script>

<style>
.title{
	display: flex;
	justify-content: center;
	margin-top: 30rpx;
}
.qrimg {
	display: flex;
	justify-content: center;
	margin-top: 150px;
}
.qrimg-i{
	margin-right: 10px;
}
slider {
	width: 100%;
}
input {
	width: 100%;
	margin-bottom: 20upx;
}
.btns {
	display: flex;
	flex-direction: column;
	width: 100%;
}
button {
	width: 100%;
	margin-top: 10upx;
}
</style>