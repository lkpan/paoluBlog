<template>
	<view class="home">
		<uni-file-picker v-model="imageValue" ref="file" title="最多上传三个" limit="3" fileMediatype="image"
			:auto-upload="false" return-type="array" mode="grid" @select="select" @progress="progress" @success="success" @fail="fail" />
			<button @click="uploadFile">上传文件</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				imageValue: [],
				filesUrl:[]
			}
		},
		onLoad() {

		},
		methods: {
			// 获取上传状态
			select(e) {
				console.log('选择文件：', e)
			},
			// 获取上传进度
			progress(e) {
				console.log('上传进度：', e)
			},

			// 上传成功
			success(e) {
				console.log(e,'成功');
			},

			// 上传失败
			fail(e) {
				console.log('上传失败：', e)
			},
			uploadFile(){
				this.$refs.file.upload()
			}
		}
	}
</script>

<style>

</style>