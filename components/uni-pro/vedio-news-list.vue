<template>
	<view class="coupon-page s-row" @tap="open(itemid)" v-bind:key='itemid' style="margin:16upx 16upx 8upx 16upx;background: white;border-radius: 32upx;height: 220px;">
		<view class="image" style="width: 100%;height: 140px;">
			<image style="width: 100%;height: 100%;" :src="itempic"></image>
		</view>
		<view class="content" style="width: 100%;">
			<view style="display: flex">
				<!-- <image v-if="logo!=null" style="margin-left:8upx;height: 24upx;width: 9%;margin-top: 8upx;margin-right: 4upx" :src="logo"></image> -->
				<view class="twolist-hidden" style="height: 80upx;font-size: 28upx;margin-right:4upx;margin-left: 2px;width: 95%;max-lines:2;overflow: hidden">
					{{ itemtitle }}
				</view>
			</view>
		</view>
		<view class="num s-row" style="font-size: 24upx;margin-top: 30px;margin-left: 14upx;margin-right: 32upx;">
			<text class="tmprice" style="width: 91%;float: left;margin-right: 0px;color:#C8C7CC;">{{itemprice}}</text>
			<image v-if="logo!=null" style="width: 9%;height: 24upx;float: left;" :src="logo"></image>
		</view>
	</view>
</template>

<script>
	export default {
		name: "vedio-news-list",
		props: [
			'isEnable', "itempic", "tkmoneys", "itemtitle", "itemprice", "itemsale", "itemendprice", "couponmoney", "itemid",
			"logo", "tkmoney", "isInvitation"
		],

		methods: {
			open(itemid) {
				let relation_id = this.$queue.getData("relation_id");
				if (relation_id) {
					uni.navigateTo({
						url: "/pages/detail/goodsinfo?itemid=" + itemid + "&relation_id=" + relation_id,
					})
				} else {
					uni.navigateTo({
						url: "/pages/detail/goodsinfo?itemid=" + itemid,
					})
				}
			},
		}
	}
</script>

<style>
	@import "../../static/css/index.css";
</style>
