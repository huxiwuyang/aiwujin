<template>
	<view class="index-content" style="background: white">
		<view class="top-bckground">
			<!-- 搜索板块 -->
			<view class="index-header" style="top: 0;">
				<view>
					<!-- #ifdef H5 -->
					<scroll-view scroll-x class="nav selectTop" scroll-with-animation :scroll-left="scrollLeft">
						<!-- #endif -->
						<!-- #ifndef H5 -->
						<scroll-view scroll-x class="nav selectTop1" scroll-with-animation :scroll-left="scrollLeft">
							<!-- #endif -->
							<view class="cu-item" style="width: 18%;" :class="index1 == TabCur ? 'text-green cur' : ''" v-for="(item, index1) in category" :key="index1"
							 @tap="tabSelect" :data-id="index1">
								<text :style="
								index1 == TabCur
									? 'background: #FFFFFF;padding: 4upx 20upx 4upx 20upx;border-radius: 32upx;margin-right: -20upx'
									: 'background: transparent;color: #FFFFFF;margin-right: -20upx'
							">
									{{ item.name }}
								</text>
							</view>
						</scroll-view>
				</view>
			</view>
			<!-- banner板块 -->
			<view v-if="TabCur==0">
				<!-- #ifndef H5 -->
				<view class="swiper" style="padding-top: 35upx;background: white">
				<!--#endif-->
				<!-- #ifdef H5 -->
				<view class="swiper" style="padding-top: 35px;background: white">
				<!--#endif-->
					<swiper class="swiper-container" :autoplay="true" :interval="4000" :circular="true" :indicator-dots="true"
					 indicator-active-color="#FC3F78" indicator-color="#cccccc" style="height: 180px;padding: 18upx 16upx 8upx 16upx">
						<block v-for="(item, index3) in banners" :key="index3">
							<swiper-item class="swiper-wrapper" @tap="toGoodsInfo(item.url)">
								<image :src="item.image_url" style="width: 100%;height: 180px;border-radius: 32upx;" mode="scaleToFill"></image>
							</swiper-item>
						</block>
					</swiper>
				</view>
					<!-- banner结束 -->

					<!-- l -->
					<view class="index-coupon has-pd-10" style="background: ghostwhite;padding-top: 2upx;">
						<view class="goods-list" v-if="category[0].orderList.length > 0">
							<vedio-news-list v-for="(item, index8) in category[0].orderList" :tkmoney="item.tkmoney" :tkmoneys="item.tkmoneys"
							 :itemid="item.itemid" :isEnable="isEnable" :is-invitation="isInvitation" :logo="logo" :itempic="item.itempic + '_310x310.jpg'"
							 :itemtitle="item.itemtitle" :itemprice="'在售价 ¥' + item.itemprice" :itemsale="item.itemsale" :itemendprice="item.itemendprice"
							 :couponmoney="item.couponmoney"></vedio-news-list>
						</view>
					</view>
				</view>
				<!-- #ifdef APP-PLUS -->
				<view class="index-coupon has-pd-10" style="margin:200upx 8upx 180upx 8upx ;background: #FFFFFF" v-if="TabCur == 1">

					<!-- #endif -->
					<!-- #ifndef APP-PLUS -->
					<view class="index-coupon has-pd-10" style="margin:90upx 8upx 180upx 8upx ;background: #FFFFFF" v-if="TabCur == 1">

						<!-- #endif -->
						<view class="category" v-if="category[TabCur].banner.length > 0">
							<view class="swiper" style="height: 180px;">
								<swiper class="swiper-container" :autoplay="true" :interval="4000" :circular="true" :indicator-dots="true"
								 indicator-active-color="#FC3F78" indicator-color="#cccccc" style="height: 100%;padding: 18upx 16upx 8upx 16upx">
									<block v-for="(item, i) in category[TabCur].banner" :key="item.son_name" >
										<swiper-item class="swiper-wrapper" style="overflow:scroll;" @tap="toCategory(item.son_name)">
											<image :src="item.imgurl" style="width: 100%;height: 180px;border-radius: 32upx;" mode="scaleToFill"></image>
											<view>{{ item.son_name }}</view>
										</swiper-item>
									</block>
								</swiper>
							</view>
						</view>
						<view class="index-coupon has-pd-10" style="background: ghostwhite;padding-top: 2upx;">
							<view class="goods-list" v-if="TabCur == 1">
								<vedio-news-list v-for="(item, index9) in category[TabCur].orderList" :tkmoney="item.tkmoney" :tkmoneys="item.tkmoneys"
								 :itemid="item.itemid" :isEnable="isEnable" :is-invitation="isInvitation" :logo="logo" :itempic="item.itempic + '_310x310.jpg'"
								 :itemtitle="item.itemtitle" :itemprice="'在售价 ¥' + item.itemprice" :itemsale="item.itemsale" :itemendprice="item.itemendprice"
								 :couponmoney="item.couponmoney"></vedio-news-list>
							</view>
						</view>
					</view>
					
					<view class="index-coupon has-pd-10" style="margin:90upx 0upx 180upx 0upx ;background: #FFFFFF" v-if="TabCur == 2">
						<view class="index-coupon has-pd-10" style="background: white;padding-top: 2upx;">
							<view class="goods-list" v-if="TabCur == 2">
								<image :src="'../../static/img/home/摄图网_500534897.jpg'" style="width: 100%;height: 150px;"></image>
							</view>
							<view style="margin-left: 20px;margin-right: 20px;height: 60px;">
								<view style="height: 30px;font-weight: bold;line-height: 30px;">标题</view>
								<editor style="border-radius: 5px;min-height: 30px;height: 30px;background-color: #EBEBEB;padding-left: 10px;line-height: 30px;"></editor>
							</view>
							<view style="margin-left: 20px;margin-right: 20px;margin-top: 10px;height: 160px;">
								<view style="height: 30px;font-weight: bold;line-height: 30px;">封面上传</view>
								<view id="img-list" style="height: 100px;">
									<image style="border-radius: 10px;width: 100px;height: 100px;background-color: #EBEBEB;"></image>
								</view>
								<view style="height: 30px;font-size: 10px;color: #C8C7CC;line-height: 30px;">仅支持JPG、JPEG、PNG、GIF每张20M之内</view>
							</view>
							<view style="margin-left: 20px;margin-right: 20px;margin-top: 10px;height: 160px;">
								<view style="height: 30px;font-weight: bold;line-height: 30px;">视频上传</view>
								<image style="border-radius: 10px;width: 100%;height: 100px;background-color: #EBEBEB;"></image>
								<view style="height: 30px;font-size: 10px;color: #C8C7CC;line-height: 30px;">上传视频过程需要一段时间请耐心等待</view>
							</view>
							<view style="border-radius: 30px;background-color: #f14b62;margin-left: 20px;margin-right: 20px;margin-top: 10px;height: 30px;text-align: center;
							line-height: 30px;color: white;">
								提交
							</view>
						</view>
					</view>

					<!-- 加载更多提示 -->
					<view v-if="TabCur != 2" class="s-col is-col-24">
						<load-more :loadingType="category[TabCur].loadingType" :contentText="contentText"></load-more>
					</view>

					<!-- 加载更多提示 -->
					<empty v-if="category[TabCur].orderList.length === 0 && category[TabCur].loadingType == 0" des="暂无数据"></empty>

					<view id="shareit" v-if="show_share" @tap="closeShare">
						<image @click="goHongbao" class="arrow" src="../../static/home.png"></image>
						<view style="border-radius: 100px;color: #FFFFFF;font-size: 18px" @tap="closeShare">X</view>
					</view>


					<view class="scroll_top" @tap="topScrollTap" v-bind:class="[scrollTop != 0 ? 'active' : '', '']" style="bottom: 56px;"><text
						 class="iconfont icon-shangla"></text></view>


					<tki-qrcode ref="qrcode" :val="erweima" :size="600" background="#fff" foreground="#000" pdground="#000" :onval="true"
					 :loadMake="true" @result="qrR" :show="false"></tki-qrcode>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import simpleModal from '@/components/simple-pro/customModal.vue';
	import tkiQrcode from '@/components/tki-qrcode/tki-qrcode.vue';

	export default {
		components: {
			simpleModal,
			tkiQrcode
		},
		onPageScroll: function(e) {
			this.scrollTop = e.scrollTop > 200;
		},
		data() {
			return {
				topH: 1000,
				logo: '../../static/img/mao.png',
				TabCur: 0,
				scrollLeft: 0,
				messageList: [],
				showEmpty: false,
				banner: [{
					id: '1'
				}],
				scrollTop: 0,
				old: {
					scrollTop: 0
				},
				category: [{
						name: '看视频',
						positon: 0,
						loadingType: 0,
						page: 0,
						orderList: [],
						banner: []
					},
					{
						name: '5视频',
						positon: 11,
						loadingType: 0,
						page: 0,
						orderList: [],
						banner: [{
								son_name: '干果',
								imgurl: 'http://img.haodanku.com/31695fbcfec8af7274b493698d5c1f5a-600'
							},
							{
								son_name: '干货',
								imgurl: 'http://img.haodanku.com/40693f1b39155f40843b4023d938a812-600'
							},
							{
								son_name: '速食',
								imgurl: 'http://img.haodanku.com/78ca01c1baddaed135f179cbf495d780-600'
							}
						]
					},
					{
						name: '视频投稿',
						positon: 10,
						loadingType: 0,
						page: 0,
						orderList: [],
						banner: [
						]
					},
				],
				banners: [{
					image_url: 'http://shegnqx.oss-cn-beijing.aliyuncs.com/1577946584839.jpg',
					url: '/pages/member/news'
				}, {
					image_url: 'http://shegnqx.oss-cn-beijing.aliyuncs.com/1577946584839.jpg',
					url: '/pages/member/news'
				}, {
					image_url: 'http://shegnqx.oss-cn-beijing.aliyuncs.com/1577946584839.jpg',
					url: '/pages/member/news'
				}, {
					image_url: 'http://shegnqx.oss-cn-beijing.aliyuncs.com/1577946584839.jpg',
					url: '/pages/member/news'
				}],
				navlist: [{
						title: '广播',
						image: '../../static/img/home/广播.png',
						url: '9'
					},
					{
						title: '区域',
						image: '../../static/img/home/乡镇.png',
						url: 'pinduoduo'
					},
					{
						title: '政务',
						image: '../../static/img/home/政务.png',
						url: 'jingdong'
					},
					{
						title: '旅游',
						image: '../../static/img/home/旅游.png',
						url: 'remeng'
					},
					{
						title: '商城',
						image: '../../static/img/home/商城.png',
						url: '4'
					}
				],
				juhuasuanlist: [],
				couponlist: [],
				dataList: [],
				page: 1,
				min_id: 1,
				min_ids: 1,
				min_ida: 1,
				erweima: '',
				cat_id: 0,
				loadingType: 0,
				index: 0,
				show_share: false,
				genderKey: 'gender',
				copyTklStatus: '',
				isInvitation: 0,
				isEnable: "否",
				relation_id: '',
				contentText: {
					contentdown: '上拉显示更多',
					contentrefresh: '正在加载...',
					contentnomore: '没有更多数据了'
				}
			};
		},
		onLoad: function() {
			let a = this.$queue.getData("isEnable")
			if (a) {
				this.isEnable = a;
			}
			this.loadBanner();
			this.loadJuhuasuanlist();
			this.loadCouponList();
			this.loadMessage();
			// this.loadInfos();
		},
		onReachBottom() {
			this.loadCouponList();
		},
		onReady: function() {
			this.loadJuhuasuanlist();
		},
		onShow() {
			// #ifdef APP-PLUS
			this.getClipboardData();
			// #endif
			this.getUserInfo()


		},

		methods: {
			/**
			 * 首页轮播
			 */
			loadBanner: function() {
				this.$Request.get('/tao/banner/user/list').then(res => {
					if (res.status === 0) {
						this.banners = res.data;
					}
				});
			},

			/**
			 * 网红抖货
			 */
			loadJuhuasuanlist: function() {
				this.$Request.get('/api/column/apikey/maxd/type/1/back/10/min_id/' + this.min_ids + '/sort/0/cid/' + this.cat_id).then(
					res => {
						if (res.code === 1) {
							this.juhuasuanlist = [];
							for (let i = this.index; i < this.index + 8; i++) {
								this.juhuasuanlist.push(res.data[i]);
							}
							this.min_ids = res.min_id;
						}
						uni.stopPullDownRefresh(); // 停止刷新
					});
			},

			/**
			 * @param {Object} type加载
			 */
			loadCouponList: function(type) {
				this.category[this.TabCur].loadingType = 1;
				this.$Request
					.get('/api/column/apikey/maxd/type/9/back/20/min_id/' + this.category[this.TabCur].page + '/sort/9/cid/' + this.category[
						this.TabCur].positon)
					.then(res => {
						this.category[this.TabCur].loadingType = 0;
						if (res.code === 1) {
							this.category[this.TabCur].page = res.min_id;
							res.data.forEach(d => {
								let grade = this.$queue.getData('grade');
								d.tkmoneys = (d.tkmoney * 0.7).toFixed(2);
								if (grade) {
									d.tkmoney = (d.tkmoney * parseFloat(grade)).toFixed(2);
								} else {
									d.tkmoney = (d.tkmoney * 0.3).toFixed(2);
								}
								d.itemsale = d.itemsale > 10000 ? '已售 ' + (d.itemsale / 10000).toFixed(1) + '万' : '已售 ' + d.itemsale;
								this.category[this.TabCur].orderList.push(d);

								// this.category[this.TabCur].orderList.push(d);
							});
						} else {
							this.category[this.TabCur].loadingType = 2;
						}
						if (type === 'Refresh') {
							uni.stopPullDownRefresh(); // 停止刷新
						}
						uni.hideLoading();
					});
			},


			getUserInfo() {
				let userId = this.$queue.getData('userId');
				if (userId) {
					this.$Request.getT('/user/' + userId).then(res => {
						if (res.status === 0) {
							this.$queue.setData('image_url', res.data.image_url);
							this.$queue.setData('mobile', res.data.phone);
							this.isInvitation = res.data.isInvitation;
							this.topH = 1050;
							if (res.data.relationId) {
								this.relation_id = res.data.relationId;
								this.topH = 850;
								
							} else {
								this.topH = 1050;
							}
							this.$queue.setData('isInvitation', res.data.isInvitation);
							this.$queue.setData('relation', res.data.invitation);
							this.$queue.setData('grade', res.data.grade);
							this.$queue.setData('nickName', res.data.nickName);
							this.$queue.setData('relation_id', res.data.relationId);
						}
					});
				}
			},
			/**
			 * 加载公告
			 */
			loadMessage() {
				this.$Request.getT('/message/page/1/0/5').then(res => {
					if (res.status === 0) {
						this.messageList = res.data.content;
					}
				});
			},

			/**
			 * 获取公众号二维码
			 */
			loadInfos() {
				let that = this;
				this.$Request.getT('/wx/token').then(res => {
					uni.request({
						url: 'https://www.gomyorder.cn/qrcode?access_token=' + res,
						method: 'POST',
						header: {
							'content-type': 'application/json'
						},
						data: {
							action_name: 'QR_LIMIT_STR_SCENE',
							action_info: {
								scene: {
									scene_str: this.$queue.getData('relation_id') ? this.$queue.getData('relation_id') : this.$queue.getInvitation()
								}
							}
						},
						success: function(result) {
							that.erweima = result.data.url;
						},
						fail: function(e) {
							console.error(e);
						}
					});
				});
			},
			/**
			 * @param {Object} e tab切换
			 */
			tabSelect(e) {
				this.TabCur = e.currentTarget.dataset.id;
				this.scrollLeft = (e.currentTarget.dataset.id - 1) * 50;
				let list = this.category[e.currentTarget.dataset.id].orderList;
				if (list.length == 0) {
					// uni.showLoading({
					// 	title: '加载中...'
					// });
					this.loadCouponList('Refresh');
				}
			},

			qrR(path) {
				this.$queue.setData('erweimas', path);
			},

			toCategory(son_name) {
				uni.navigateTo({
					url: '/pages/categray/search?cid=' + this.category[this.TabCur].positon + '&name=' + son_name
				});
			},

			//app查询
			goSearch() {
				uni.navigateTo({
					url: '/pages/search/search?keywords=' + this.copyTklStatus
				});
				this.$refs.simpleModalTkls.hide();
				this.copyTklStatus = '';
				uni.setClipboardData({
					data: '',
					success: r => {
						uni.showToast({
							icon: 'none',
							title: '搜索中',
							duration: 1500
						});
					}
				});
			},
			//app查询弹框
			TklmaskClose: function(e) {
				this.$refs.simpleModalTkls.hide();
				this.copyTklStatus = '';
				uni.setClipboardData({
					data: '',
					success: r => {
						uni.showToast({
							icon: 'none',
							title: '已取消',
							duration: 1500
						});
					}
				});
			},
			TklmaskCloses: function(e) {
				this.$refs.simpleModalTkls.hide();
			},
			//获取剪切板
			async getClipboardData() {
				let that = this;
				uni.getClipboardData({
					success: function(res) {
						that.copyTklStatus = res.data;
						if (res.data) {
							if (res.data.indexOf('http') == -1 && res.data.indexOf('&') == -1 && res.data.indexOf('￥') == -1) {
								that.$refs.simpleModalTkls.show({
									showConfirmButton: false
								});
							} else {
								that.$refs.simpleModalTkls.hide();
							}
						} else {
							that.$refs.simpleModalTkls.hide();
						}
					},
					fail: function(res) {}
				});
			},
			/**
			 * 会员授权界面
			 */
			goPublisherInfo() {
				let token = this.$queue.getData('token');
				let userId = this.$queue.getData('userId');
				if (token) {
					uni.navigateTo({
						url: '/pages/member/publisher?uid=' + userId + '&token=' + token
					});
				} else {
					this.loginS();
				}
			},
			/**
			 * 登录检测
			 */
			loginS() {
				//#ifdef H5
				uni.showModal({
					title: '登录提醒',
					showCancel: false,
					content: '当前账号未登录\n请登录后申请',
					success: confirmRes => {
						if (confirmRes.confirm) {
							uni.navigateTo({
								url: '/pages/member/register'
							});
						}
					}
				});
				//#endif
				//#ifndef H5
				uni.showModal({
					title: '登录提醒',
					showCancel: false,
					content: '当前账号未登录\n请登录后申请',
					success: confirmRes => {
						if (confirmRes.confirm) {
							uni.navigateTo({
								url: '/pages/public/login'
							});
						}
					}
				});
				//#endif
			},
			goWebView(item) {
				if (item.type == 'url') {
					//#ifndef H5
					uni.navigateTo({
						url: '/pages/member/ping?url=' + item.url
					});
					//#endif
					//#ifdef H5
					window.location.href = item.url;
					//#endif
				}
			},

			/**
			 * 快速置顶
			 */
			topScrollTap: function() {
				uni.pageScrollTo({
					scrollTop: 0,
					duration: 300
				});
			},

			/**
			 * @param {Object} url
			 * @param {Object} titles 首页item跳转
			 */
			toNavList: function(url, title) {
				if (url === 'remeng') {
					uni.navigateTo({
						url: '/pages/hot/index'
					});
				} else if (url === 'pinduoduo') {
					uni.navigateTo({
						url: '/pages/pdd/index'
					});
				} else if (url === 'shoucang') {
					uni.navigateTo({
						url: '/pages/footer/like'
					});
				} else if (url === 'jingdong') {
					uni.navigateTo({
						url: '/pages/jd/jds'
					});
				} else if (url === 'rexiao') {
					uni.switchTab({
						url: '/pages/hot/index'
					});
				} else if (url === 'daequan') {
					uni.navigateTo({
						url: '/pages/index/mian'
					});
				} else if (url === 'meishi') {
					uni.navigateTo({
						url: '/pages/index/food?title=' + title + '&type=9'
					});
				} else if (url === 4) {
					uni.navigateTo({
						url: '/pages/index/tuiguang?cid=4'
					});
				} else if (url === 3) {
					uni.navigateTo({
						url: '/pages/index/nine?title=' + title + '&type=3'
					});
				} else if (url === 2) {
					uni.navigateTo({
						url: '/pages/index/tuiguang?cid=10'
					});
				} else if (url === 5) {
					uni.navigateTo({
						url: '/pages/index/tuiguang?cid=1'
					});
				} else if (url === 'gengduofenlie') {
					// #ifdef H5
					window.location.href = 'https://www.gomyorder.cn/help/news.html';
					// #endif
					// #ifndef H5
					uni.navigateTo({
						url: '/pages/member/webview?url=https://www.gomyorder.cn/help/news.html'
					});
					// #endif
				} else if (url === 'gaoyongjingxuan') {
					uni.navigateTo({
						url: '/pages/index/tuiguang?cid=9'
					});
				} else if (url == 22) {
					uni.navigateTo({
						url: '/pages/cate/index'
					});
				} else {
					uni.navigateTo({
						url: '/pages/index/list?title=' + title + '&type=' + url
					});
				}
			},
			toGoodsInfo: function(url) {
				if (url.indexOf('/pages/') !== -1) {
					uni.navigateTo({
						url
					});
				} else {
					//#ifndef H5
					uni.navigateTo({
						url: '/pages/member/webview?url=' + url
					});
					//#endif
					//#ifdef H5
					window.location.href = url;
					//#endif
				}
			},
			toGoodsInfos: function(itemid) {
				let relation_id = this.$queue.getData('relation_id');
				if (relation_id) {
					uni.navigateTo({
						url: '/pages/detail/goodsinfo?itemid=' + itemid + '&relation_id=' + relation_id
					});
				} else {
					uni.navigateTo({
						url: '/pages/detail/goodsinfo?itemid=' + itemid
					});
				}
			},
			toSearchIndex: function() {
				uni.navigateTo({
					url: '/pages/homeSearch/index'
				});
			},
			toZujiIndex: function() {
				uni.navigateTo({
					url: '/pages/footer/index'
				});
			}
		},
	};
</script>

<style lang="scss">
	@import '../../static/css/index.css';

	.img-gender {
		/* #ifndef H5 */
		width: 80upx;
		height: 80upx;
		margin-top: 60upx
			/* #endif */
			/* #ifdef H5 */
			width: 60upx;
		height: 60upx;
		margin-top: 20upx
			/* #endif */

	}

	.top-background {
		background: -webkit-linear-gradient(left, #f15b6c 0, #ff6db2 100%);
		background: -o-linear-gradient(left, #f15b6c 0, #ff6db2 100%);
		background: -ms-linear-gradient(left, #f15b6c 0, #ff6db2 100%);
		background: -webkit-gradient(linear, right top, left top, color-stop(0, #f15b6c), to(#ff6db2));
		background: -o-linear-gradient(right, #f15b6c 0, #ff6db2 100%);
		background: linear-gradient(to left, #f15b6c 0, #ff6db2 100%);
	}

	.swiper-item img {
		display: block;
	}

	.title .fr {
		float: right;
		line-height: 50px;
		margin-right: 16px;
		font-size: 10px;
		color: #ccc;
	}

	/*#ifndef APP-PLUS*/
	.scroll_top_act {
		background: white;
		top: 45px;
		position: fixed;
	}

	/*#endif*/
	/*#ifdef APP-PLUS*/
	.scroll_top_act {
		background: white;
		top: 65px;
		position: fixed;
	}

	/*#endif*/

	.banner {
		border-radius: 10px;
		margin: 8px 8px 0 8px;
		overflow: hidden;
		display: flex;
	}

	.banner img {
		width: 100%;
	}

	.banner>.left {
		flex: 4;
		/* margin-right: 10upx; */
		border-right: 2px solid #f2f2f2;
		overflow: hidden;
	}

	.banner>.right {
		flex: 7;
	}

	.right .top {
		width: 100%;
		/* margin-bottom: 7upx; */
		/*border-bottom: 2px solid #f2f2f2;*/
		overflow: hidden;
	}

	.right .bottom {
		display: flex;
		width: 100%;
	}

	.right .bottom .bottom-left {
		flex: 6;
		/* margin-right: 5upx; */
		overflow: hidden;
		border-right: 1px solid #f2f2f2;
	}

	.right .bottom .bottom-right {
		flex: 6;
		/* margin-left: 5upx; */
		/* border-left: 1px solid #f2f2f2; */
		overflow: hidden;
	}

	.marquee-box {
		border-radius: 5px;
		overflow: hidden;
		position: relative;
		background: #fff;
		height: 26px;
		line-height: 26px;
	}

	.marquee-title {
		padding-left: 8px;
		padding-right: 8px;
		position: absolute;
		color: #ff5100;
		top: 0;
		left: 0;
		z-index: 3;
		background: #fff;
		font-size: 14px;
	}

	.marquee {
		padding: 6px 10px;
		color: #000;
		display: inline-block;
		white-space: nowrap;
		animation: 35s wordsLoop linear infinite normal;
		font-size: 14px;
	}

	@keyframes wordsLoop {
		0% {
			transform: translateX(350px);
			-webkit-transform: translateX(350px);
		}

		100% {
			transform: translateX(-100%);
			-webkit-transform: translateX(-100%);
		}
	}

	.selectTop {
		z-index: 100;
		padding-left: 16upx;
		padding-right: 16upx;
		position: fixed;
		background: -webkit-linear-gradient(left, #f15b6c 0, #ff6db2 100%);
		background: -o-linear-gradient(left, #f15b6c 0, #ff6db2 100%);
		background: -ms-linear-gradient(left, #f15b6c 0, #ff6db2 100%);
		background: -webkit-gradient(linear, right top, left top, color-stop(0, #f15b6c), to(#ff6db2));
		background: -o-linear-gradient(right, #f15b6c 0, #ff6db2 100%);
		background: linear-gradient(to left, #f15b6c 0, #ff6db2 100%);
	}

	.selectTop1 {
		z-index: 999;
		padding-left: 16upx;
		padding-right: 16upx;
		position: fixed;
		top: 130upx;
		background: -webkit-linear-gradient(left, #f15b6c 0, #ff6db2 100%);
		background: -o-linear-gradient(left, #f15b6c 0, #ff6db2 100%);
		background: -ms-linear-gradient(left, #f15b6c 0, #ff6db2 100%);
		background: -webkit-gradient(linear, right top, left top, color-stop(0, #f15b6c), to(#ff6db2));
		background: -o-linear-gradient(right, #f15b6c 0, #ff6db2 100%);
		background: linear-gradient(to left, #f15b6c 0, #ff6db2 100%);
	}

	#shareit {
		-webkit-user-select: none;
		position: fixed;
		width: 100%;
		height: 2000px;
		background: rgba(0, 0, 0, 0.85);
		text-align: center;
		top: 0;
		left: 0;
		z-index: 999;
	}

	#shareit img {
		max-width: 100%;
	}

	.arrow {
		width: 80%;
		height: 420px;
		margin-top: 100px;
	}

	.share_close {
		position: absolute;
		bottom: -0.5rem;
		left: 50%;
		margin-left: -0.3rem;
		width: 0.6rem;
		height: 0.6rem;
		background: url(http://img.haodanku.com/Fo2-nJ_43fsFStbAfqMUEcCFJnJ6);
		background-size: 100% 100%;
		cursor: pointer;
	}

	#follow {
		margin-right: 60px;
		margin-left: 30px;
		width: 90%;
		height: 50px;
		line-height: 50px;
		text-align: left;
		text-decoration: none;
		font-size: 18px;
		color: white;
		float: left;
		margin-top: 160px;
	}

	.category {
		padding: 4upx;

		.list {
			margin-top: 20upx;
			width: 100%;
			display: flex;
			flex-wrap: wrap;

			.box {
				width: 20%;
				margin-bottom: 20upx;
				display: flex;
				justify-content: center;
				align-items: center;
				flex-wrap: wrap;

				image {
					width: 60%;
					height: calc(71.44vw / 3 * 0.6);
				}

				.text {
					margin-top: 5upx;
					width: 100%;
					display: flex;
					justify-content: center;
					font-size: 26upx;
				}
			}
		}
	}

	.swiper-box {
		height: calc(100% - 40px);
	}
	
.swiper-wrapper view{
	position: absolute;
	left: 0;
	right: 0;
	bottom: 9%;
	line-height: 60rpx;
	text-align: center;
	color: #FFFFFF;
	z-index: 999;
}
	
</style>
