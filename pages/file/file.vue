<template>
	<view>
		<view class="uploadGroup">
			<view class="box" v-for="(item,index) in tempFiles">
				<image :src="item.path" mode="aspectFill" @click="preImg(index)">

				</image>
				<view class="close" @click="onClose(index)">
					X
				</view>
			</view>
			<view class="box add" @click="addPic" v-show="tempFiles.length<9">
				+
			</view>

		</view>
		<button @click="submitBtn">点击上传</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				tempFiles: [],
				picArr:[]  // 存储图片地址
			};
		},
		methods: {
			// 点击上传
			submitBtn() {
				let newArr = this.tempFiles.map(async (item)=>{
					return await this.uploadFun(item)
				})
				// 监听promise执行进程
				Promise.all(newArr).then(res=>{
					let arr = res.map(item=>{
						return item.fileID
					})
					this.picArr = arr
				})
			},
			// 上传功能,返回一个promise对象
			uploadFun(item) {
				return uniCloud.uploadFile({
					filePath: item.path,
					cloudPath: item.name
				})
			},
			// 预览图片
			preImg(index) {
				uni.previewImage({
					urls: this.tempFiles.map(item => item.path), // 把地址重新生成数组
					current: index
				})
			},
			// 添加图片
			addPic() {
				uni.chooseImage({
					success: res => {
						this.tempFiles = [...this.tempFiles, ...res.tempFiles]
						if (this.tempFiles.length > 10) {
							this.tempFiles = this.tempFiles.slice(0, 9)
						}
					}
				})
			},
			// 删除某张图片
			onClose(index) {
				this.tempFiles.splice(index, 1)
			}
		}
	}
</script>

<style lang="scss">
	.uploadGroup {
		display: flex;
		padding: 50rpx;
		flex-wrap: wrap;

		.box {

			position: relative;
			width: 200rpx;
			height: 200rpx;
			background: #eee;

			image {
				width: 100%;
				height: 100%;

			}

			.close {
				position: absolute;
				top: 0;
				right: 0;
				width: 50rpx;
				height: 50rpx;
				background: rgba(0, 0, 0, .7);
				color: #fff;
				border-radius: 0 0 0 80rpx;
				text-align: center;
			}
		}

		.add {
			font-size: 80rpx;
			display: flex;
			align-items: center;
			justify-content: center;
		}
	}
</style>