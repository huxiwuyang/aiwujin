<template>
	<view>
		<view style="width: 100%;height: 40px;border-bottom: solid 1px #d9d9d9;">
			<view style="float: left;font-weight: bolder;margin-top: 10px;margin-left: 10px;padding-left: 5px;height: 20px;border-left: solid 3px #FF6DB2;">
				广播频道
			</view>
		</view>
		<view style="float: left;width: 100%;height: 100px;">
			<view style="width: 50%;height: 100px;float: left;">
				<image src="../../static/img/886.jpg" style="width: 90%;height: 90%;margin-top: 5px;margin-left: 5%;">
					
				</image>
			</view>
			<view style="width: 50%;height: 100px;float: left;">
				<view style="width: 100%;height: 30px;line-height: 30px;font-weight: bolder;font-size: 15px;">FM886生活广播</view>
				<view style="width: 100%;height: 30px;">
					<view style="color: #FFFFFF;border-radius: 15px;background-color: red;width: 30px;height: 15px;
					float: left;font-size:1px;line-height: 16px;padding-left: 2px;margin-top: 2px;">
						LIVE
					</view>
					<view style="height: 20px;margin-left: 5px;float: left;color: #b5b5b5;">
						乐居常州
					</view>
				</view>
				<view style="width: 100%;height: 40px;font-size: 10px;color: #b5b5b5;
				line-height: 60px;">
				08:00-09:00
				</view>
			</view>
		</view>
		<view style="float: left;margin-top: 10px;width: 100%;height: 40px;border-bottom: solid 1px #d9d9d9;">
			<view style="font-weight: bolder;margin-top: 10px;margin-left: 10px;padding-left: 5px;height: 20px;border-left: solid 3px #FF6DB2;">
				广播精品
			</view>
		</view>
		<view class="goods-list" v-if="category[0].orderList.length > 0">
			<broadcast-card v-for="(item, index8) in category[0].orderList" :tkmoney="item.tkmoney" :tkmoneys="item.tkmoneys"
			 :itemid="item.itemid" :isEnable="isEnable" :is-invitation="isInvitation" :logo="logo" :itempic="item.itempic + '_310x310.jpg'"
			 :itemtitle="item.itemtitle" :itemprice="'在售价 ¥' + item.itemprice" :itemsale="item.itemsale" :itemendprice="item.itemendprice"
			 :couponmoney="item.couponmoney"></broadcast-card>
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
						name: '热门',
						positon: 0,
						loadingType: 0,
						page: 0,
						orderList: [],
						banner: []
					},
					{
						name: '武进',
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
						name: '时政',
						positon: 10,
						loadingType: 0,
						page: 0,
						orderList: [],
						banner: [{
								son_name: '卫生巾',
								imgurl: 'http://img.haodanku.com/d4ad5258247d5cfcd397fed061c55332-600'
							},
							{
								son_name: '卷纸',
								imgurl: 'http://img.haodanku.com/1fa08dd194ae423de72af366e6fa319c-600'
							},

							{
								son_name: '抽纸',
								imgurl: 'http://img.haodanku.com/8b15fc81e69e7140bfdd6af51890a5f7-600'
							}
						]
					},
					{
						name: '社会',
						positon: 4,
						loadingType: 0,
						page: 0,
						orderList: [],
						banner: [{
								son_name: '卸妆',
								imgurl: 'http://img.haodanku.com/11f4c31e57040ca6578e395764685f9d-600'
							},
							{
								son_name: '唇膏',
								imgurl: 'http://img.haodanku.com/73dce1ba7e1e6c2f087a82e6e2daaf0f-600'
							},

							{
								son_name: '洗面奶',
								imgurl: 'http://img.haodanku.com/0390ae3565930d395244524603d38605-600'
							}
						]
					},
					{
						name: '生活',
						positon: 1,
						loadingType: 0,
						page: 0,
						orderList: [],
						banner: [{
								son_name: '妈妈装',
								imgurl: 'http://img.haodanku.com/cf445d5d9ddad49a38c0e542be22b565-600'
							},

							{
								son_name: 'T恤',
								imgurl: 'http://img.haodanku.com/397fc31d9f3abdef5177ab1ec82a254c-600'
							},
							{
								son_name: '一字肩',
								imgurl: 'http://img.haodanku.com/f4ca5e271d74fd5c29d051c7b1106f04-600'
							}
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
						url: 'broadcast'
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
				if (url === 'broadcast') {
					uni.navigateTo({
						url: '/pages/broadcast/index'
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


<style>

</style>
