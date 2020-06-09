<template>
	<view class="index-coupon has-pd-10" style="background: white;padding-top: 2upx;">
		<view class="goods-list">
			<image :src="'../../static/img/home/报料平台.jpg'" style="width: 100%;height: 150px;"></image>
		</view>
		<view style="margin-left: 20px;margin-right: 20px;height: 60px;">
			<view style="height: 30px;font-weight: bold;line-height: 30px;">标题 （选填）：</view>
			<editor style="border-radius: 5px;min-height: 30px;height: 30px;background-color: #EBEBEB;padding-left: 10px;line-height: 30px;"></editor>
		</view>
		<view style="margin-left: 20px;margin-right: 20px;margin-top: 10px;height: 160px;">
			<view style="height: 30px;font-weight: bold;">
				<view style="float: left;color: red;font-size: 20px;line-height: 35px;margin-right: 3px;">*</view>
				<view style="float: left;line-height: 30px;">描述您的问题 （必填）：</view>
		</view>
			<view id="img-list" style="height: 100px;">
				<editor placeholder="具体内容 (必填,比如时间、地点、人物)" style="border-radius: 5px;min-height: 100px;width: 100%;height: 100px;background-color: #EBEBEB;
				line-height: 30px;padding-left: 10px;"></editor>
			</view>
			<view style="height: 30px;font-size: 10px;color: #C8C7CC;line-height: 30px;">仅支持JPG、JPEG、PNG、GIF每张20M之内</view>
		</view>
		<view style="margin-left: 20px;margin-right: 20px;height: 60px;">
			<view style="height: 30px;font-weight: bold;line-height: 30px;">姓名 （昵称） （选填）：</view>
			<editor style="border-radius: 5px;min-height: 30px;height: 30px;background-color: #EBEBEB;padding-left: 10px;line-height: 30px;"></editor>
		</view>
		<view style="margin-left: 20px;margin-right: 20px;height: 60px;">
			<view style="height: 30px;font-weight: bold;line-height: 30px;">你的手机号或微信号 （选填）：</view>
			<editor style="border-radius: 5px;min-height: 30px;height: 30px;background-color: #EBEBEB;padding-left: 10px;line-height: 30px;"></editor>
		</view>
		<view style="border-radius: 30px;background-color: #f14b62;margin-left: 20px;margin-right: 20px;margin-top: 10px;height: 30px;text-align: center;
		line-height: 30px;color: white;margin-top: 40px;">
			提交
		</view>
	</view>
</template>

<script>
</script>

<style>
</style>
