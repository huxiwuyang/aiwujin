<template>
	<view class="s-page-wrapper">
		<view class="index-goods" v-if="goods&&goods.imageInfo">
			<view class="swiper">
				<swiper class="swiper-container" :autoplay="true" :interval="4000" :circular="true" :indicator-dots="true"
				 indicator-active-color="#FC3F78" indicator-color="#cccccc">
					<block v-for="(item,index) in goods.imageInfo.imageList" :key="index">
						<swiper-item class="swiper-wrapper">
							<image :src="item.url" mode="widthFix" class="is-response" lazy-load="true" :data-index="index" @tap="previewPhoto"></image>
						</swiper-item>
					</block>
				</swiper>
			</view>

			<view style="background: rgba(0, 0, 0, 0.7);padding: 8px;display: flex;font-size: 14px;font-weight: bold" v-if="goods&&!relation_id&&isEnable!='否'"
			 @tap="goPublisher">
				<view style="color: #ffd465;width: 76%;line-height: 60upx">成为会员最高可返现金￥{{ shengji }}元</view>
				<view style="width: 24%;text-align: center;font-size: 28upx;" class="b-btn">
					立即加入
					<text class="cuIcon cuIcon-right"></text>
				</view>
			</view>
			<view style="background: rgba(0, 0, 0, 0.7);padding: 8px;display: flex;font-size: 14px;font-weight: bold" v-if="goods&&relation_id && grade !== '0.7'&&isEnable!='否'"
			 @tap="shengJiMethod">
				<view style="color: #ffd465;width: 76%;line-height: 60upx;font-size: 28upx">预估返￥ {{ money }}元，升级最高返￥ {{ shengji }}元
				</view>
				<view style="width: 24%;text-align: center;font-size: 28upx;" class="b-btn">
					立即升级
					<text class="cuIcon cuIcon-right"></text>
				</view>
			</view>
			<view style="background: rgba(0, 0, 0, 0.7);padding: 8px;display: flex;font-size: 14px;font-weight: bold" v-if="goods&&grade === '0.7'&&isEnable!='否'">
				<view style="color: #ffd465;width: 75%">已成为高级会员，预估返现￥ {{ money }}元</view>
			</view>
			<view class="goods_info" style="padding-top: 6px;padding-bottom: 10px">
				<view style="padding: 8px">
					<image style="height: 26upx;width:26upx;margin-top: 4px;margin-right: 4px" src="../../static/img/jd.png"></image>
					<text style="font-weight: bold">
						<text style="color: #FC3F78;font-weight: bold" v-if="goods.shopname">【{{ goods.shopname }}】
						</text>
						<text style="color: #FC3F78;font-weight: bold" v-if="goods.shopname">【{{ goods.shopname }}】
						</text>
						<text @longpress="copyTitle">{{ goods.skuName }}</text>
					</text>
				</view>
				<view class="coupon-price s-row">
					<view class="price" style="width: 70%">
						<text style="font-size: 14px">
							券后价
							<text style="font-size: 12px">￥</text>
						</text>
						<text style="font-weight: 700;" v-if="goods.couponInfo&&goods.couponInfo.couponList.length>0">{{goods.itemendprice}}</text>
						<text style="font-size: 12px;color: grey;text-decoration:line-through;margin-left: 8px">￥{{
                            goods.itemprice}}
						</text>
					</view>
					<view class="volume" style="padding-top: 14rpx;font-size: 14px;width: 30%">{{ goods.itemsale }}
					</view>
				</view>

				<view class="coupon-price s-row"></view>
			</view>

			<view class="goods_quan s-row" style="padding-top: 8px;padding-bottom: 8px">
				<view class="row getGoodsLink">
					<view class="is-col-16 money">
						<view style="font-weight: bold" v-if="goods.couponInfo&&goods.couponInfo.couponList.length>0">
							<text>{{ goods.couponmoney }}</text>
							元优惠券
						</view>
						<view style="font-weight: bold;margin-top: 18upx;" v-else>
							优惠券已被抢光
						</view>
						<view class="date-coupon" v-if="goods.couponStartTime">使用期限: {{ goods.couponStartTime }} - {{
                            goods.couponEndTime }}
						</view>
					</view>
					<view class="is-col-8 name">
						<text @tap="shopCartShare('quan')">{{goods.couponInfo&&goods.couponInfo.couponList.length==0?'原价购买':'立即抢券'}}</text>
					</view>
				</view>
				<image lazy-load src="../../static/img/goods/goods_quan.png" mode="widthFix" class="is-response"></image>
			</view>
			<view class="goods_reco" style="margin-top: 10px;display: flex">
				<view class="goods-info-title" style="color: #FC3F78;width: 20%;text-align: center">必买理由</view>
				<view class="goods_desc" style="padding-bottom: 8px;padding-top:8px;width: 80%">{{ goods.skuName }}
				</view>
			</view>
			<view class="goods_reco" style="margin-top: -100rpx" v-if="goods.imageInfo.imageList">
				<view class="goods-info-title">宝贝详情</view>
				<view class="imglist">
					<block v-for="(item, index) in goods.imageInfo.imageList">
						<image lazy-load :src="item.url" mode="widthFix" class="is-response" @longpress="toSave(item.url)"></image>
					</block>
				</view>
			</view>
			<!-- 购买按钮 -->
			<view class="goods_shop_cart">
				<view class="cent" style="display: flex">
					<view style="text-align: center;width: 25%;margin-top: 4px;margin-bottom: 4px;display: flex">
						<view style="width: 50%;text-align: center">
							<view class="iconfont icon-shouye" @tap="goBackUp()"></view>
							<view style="font-size: 12px">首页</view>
						</view>
						<view style="width: 50%;text-align: center">
							<view class="iconfont icon-shoucang" :style="collect.isCollect" @tap="clickCollect()"></view>
							<view style="font-size: 12px" :style="collect.isCollect">{{ collect.name }}</view>
						</view>
					</view>
					<view style="width: 75%;display: flex;color: white">
						<view style="width: 50%;background: #FF6DB2;text-align: center;padding-top:8px;margin: 4px" @tap="shopCartShare('fanxian')">{{ buyDes }}
						</view>
						<view class="getTbk" style="width: 50%;background:#F15B6C; text-align: center;padding-top: 8px;margin: 4px" @tap="shopCartShare('quan')">{{ getQun }}
						</view>
					</view>
				</view>
			</view>
			<view class="navBarButtonBox">
				<!-- 顶部右侧菜单 -->
				<view v-if="navBarButton" class="goods_shop_cart_bg navBarButton" @tap="showShopCartBg('nav')"
				 @touchmove.stop.prevent="moveHandleStop"></view>
				<view class="h_newlit" v-bind:class="[navBarButton ? 'active' : '', '']">
					<view class="em">
						<view style="font-size: 14px" @tap="navBarButtontO('home')">
							<text class="iconfont icon-shouye"></text>
							返回首页
						</view>
						<view style="font-size: 14px" @tap="navBarButtontO('search')">
							<text class="iconfont icon-sousuo"></text>
							超级搜索
						</view>
						<view style="font-size: 14px" @tap="navBarButtontO('footer')">
							<text class="iconfont icon-zuji"></text>
							我的足迹
						</view>
						<view style="font-size: 14px" @tap="navBarButtontO('like')">
							<text class="iconfont icon-shoucang"></text>
							个人中心
						</view>
					</view>
				</view>
			</view>
		</view>
		<!-- 淘口令分享 -->
		<simpleModal ref="simpleModalTkl" @maskClose="TklmaskClose">
			<view class="buy-box-title">复制分享文案</view>
			<view class="buy-box-center">
				<view class="code-cent">
					<div class="cente-text">
						<div>
							<view class="textarea">
								【京东】{{ goods.skuName }}
								<br />
								【在售价】{{goods.itemprice}}元
								<br />
								【券后价】{{goods.couponmoney}}元
								<br />
								【下单链接】{{ erweima }}
								<br />
							</view>
						</div>
					</div>
				</view>
				<view class="buy-btn-copy" v-bind:class="[copyTklStatus ? 'active' : '', '']" @tap="copyTklWenAns">
					{{ copyTklStatus ? '已复制到剪切板' : '一键复制' }}
				</view>
			</view>
		</simpleModal>
		<!-- 淘口令分享 -->
		<view class="" v-if="!loadGoods">
			<view class="s-page">
				<view class="is-100vh is-flex is-column is-justify-center is-align-center ">
					<image src="../../static/img/load.gif" style="width:75px;height: 75px;" class="is-response" mode="widthFix"></image>
				</view>
			</view>
		</view>
		<view class="scroll_top active " @click="TklmaskShow()" style="bottom: 120upx;width:200upx;right: -24upx;padding-right: 20upx">
			<text>
				<text class="cuIcon cuIcon-share"></text>
				分享好友
			</text>
		</view>
		<view class="scroll_top active " @click="showModal()" style="bottom: 200upx;width: 200upx;right: -24upx;padding-right: 20upx">
			<text>
				<text class="cuIcon cuIcon-calendar"></text>
				生成海报
			</text>
		</view>
		<view class="scroll_top active " @click="bijiage()" style="bottom: 280upx;width: 200upx;right: -24upx;padding-right: 20upx">
			<text>
				<text class="cuIcon cuIcon-recharge"></text>
				我要比价
			</text>
		</view>
		<tki-qrcode ref="qrcode" :val="erweima" :size="200" background="#fff" foreground="#000" pdground="#000" :onval="true"
		 :loadMake="true" @result="qrR" :show="false"></tki-qrcode>
		<view class="cu-modal" :class="modalName=='Image'?'show':''" @tap="hideModal">
			<view class="cu-dialog" v-if="goods&&haibaoShow&&erweimapath" @tap="hideModal">
				<view class="bg-img">
					<wm-poster @success="posterSuccess" :imgSrc="goods.itempic" :QrSrc="erweimapath" :Title="goods.skuName" :PriceTxt="goods.itemendprice"
					 :OriginalTxt="goods.itemprice" :LineType='false'></wm-poster>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import simpleModal from '../../components/simple-pro/customModal.vue';
	import _app from '../../js_sdk/QuShe-SharerPoster/QS-SharePoster/app.js';
	import getSharePoster from '../../js_sdk/QuShe-SharerPoster/QS-SharePoster/QS-SharePoster.js';
	import wmPoster from '@/components/wm-poster/wm-poster.vue';
	import tkiQrcode from '@/components/tki-qrcode/tki-qrcode.vue';
	export default {
		data() {
			return {
				erweima: '',
				erweimapath: '',
				modalName: '',
				logo: '../../static/img/mao.png',
				taobao: '../../static/img/taobao.png',
				buyDes: '口令购买',
				collect: {
					name: '收藏',
					isCollect: ''
				},
				show_share: false,
				shengji: '',
				getQun: '立即领券',
				follow: false,
				loadGoods: true,
				goods: '',
				taoBaoBuy: true,
				shopCartBg: false,
				scrollTop: false,
				couponlist: [],
				navBarButton: false,
				copyTklStatus: false,
				save: false,
				tkl: '',
				relation_id: null,
				money: 0,
				grade: '',
				isInvitation: 0,
				showFollow: false,
				isWeiXin: false,
				footprintKey: 'orange-sqx-footprint-jd',
				collectKey: 'orange-sqx-collect-jd',
				poster: {},
				coupon_click_url: '',
				qrShow: false,
				haibaoImg: null,
				haibaoShow: false,
				isEnable: '否',
				canvasId: 'default_PosterCanvasId'
			};
		},
		components: {
			simpleModal,
			tkiQrcode,
			wmPoster
		},
		onShow: function() {
			this.copyTklStatus = false;
			let relation_id = this.$queue.getData('relation_id');
			if (relation_id) {
				this.relation_id = relation_id;
			}

		},
		onLoad: function(e) {
			let jd_list = this.$queue.getData("jd_list")
			if (!jd_list) {
				this.goBackUp();
				return;
			}
			let a = this.$queue.getData("isEnable")
			if (a) {
				this.isEnable = a;
			}
			let userId = this.$queue.getData('userId');
			if (userId) {
				this.$Request.getT('/user/' + userId).then(res => {
					if (res.status === 0) {
						if (res.data.isInvitation) {
							this.isInvitation = res.data.isInvitation;
							this.$queue.setData('isInvitation', res.data.isInvitation);
						}
						this.$queue.setData('relation', res.data.invitation);
						this.$queue.setData('grade', res.data.grade);
						this.$queue.setData('pddpid', res.data.pdd);
						this.$queue.setData('jdpid', res.data.jd);
						if (res.data.image_url) {
							this.$queue.setData('image_url', res.data.image_url);
						}
						this.$queue.setData('mobile', res.data.phone);
						this.$queue.setData('nickName', res.data.nickName);
						this.$queue.setData('relation_id', res.data.relationId);
						this.$queue.setData('gender', parseInt(res.data.gender));
					}
				});
			}
			this.loadGoodsInfo();
		},
		onPageScroll: function(e) {
			this.scrollTop = e.scrollTop > 200;
		},
		onNavigationBarButtonTap: function() {
			this.navBarButton = !this.navBarButton;
		},
		methods: {
			posterSuccess(haibaoImg) {
				this.haibaoImg = haibaoImg;
				this.modalName = "Image"
			},
			showModal() {
				if (!this.haibaoImg) {
					this.haibaoShow = true;
					this.$queue.showLoading("海报生成中...")
				} else {
					this.modalName = "Image"
				}

			},
			hideModal() {
				this.modalName = null
			},
			bijiage() {
				uni.navigateTo({
					url: "../bijia/index?keywords=" + this.goods.goodsName
				})

			},
			TklmaskClose: function(e) {
				this.$refs.simpleModalTkl.hide();
				this.copyTklStatus = false;
			},
			TklmaskShow: function(e) {
				this.$refs.simpleModalTkl.show({
					showConfirmButton: false
				});
			},
			shengJiMethod() {
				uni.navigateTo({
					url: '/pages/member/yao'
				});
			},
			qrR(path) {
				this.erweimapath = path;
			},
			/* 预览照片 */
			previewPhoto(e) {
				let index = e.currentTarget.dataset.index;
				let list = this.goods.imageInfo.imageList.map((item, index) => {
					return item.url;
				});
				uni.previewImage({
					current: list[index],
					urls: list,
					longPressActions: {
						itemList: ['保存图片'],
						success: function(res) {
							uni.getImageInfo({
								src: list[res.index],
								success: function(image) {
									console.log('图片信息：', JSON.stringify(image));
									uni.saveImageToPhotosAlbum({
										filePath: image.path,
										success: function() {
											uni.showToast({
												title: '图片保存成功',
												icon: 'none',
												duration: 3000
											});
										}
									});
								}
							});
						}
					}
				});
			},
			//初始化加载详情数据
			loadGoodsInfo: function() {
				let that = this;
				let item = JSON.parse(this.$queue.getData("jd_list"))
				let couponInfo = item.couponInfo.couponList[0]
				if (couponInfo.useEndTime && couponInfo.useStartTime) {
					item.couponStartTime = this.formatDate(new Date(couponInfo.useStartTime));
					item.couponEndTime = this.formatDate(new Date(couponInfo.useEndTime));
				}
				item.itempic = "https://" + item.imageInfo.imageList[0].url.split('://')[1];
				that.goods = item;
				//获取京东优惠券地址
				this.getJDUrl()
				let grade = this.$queue.getData('grade');
				if (grade) {
					that.money = (item.commissionInfo.couponCommission * parseFloat(grade)).toFixed(2);
				} else {
					that.money = (item.commissionInfo.couponCommission * 0.3).toFixed(2);
				}
				that.shengji = (item.commissionInfo.couponCommission * 0.7).toFixed(2);
				that.itemprice = item.priceInfo.price
				that.itemendprice = (item.priceInfo.price - item.couponInfo.couponList[0].discount).toFixed(2)
				that.grade = grade;
				let isExist = this.$queue.isExistJd(this.collectKey, item.skuId);
				if (isExist === true) {
					this.collect.name = '已收藏';
					this.collect.isCollect = 'color: #FC3F78';
				} else {
					this.collect.name = '收藏';
					this.collect.isCollect = false;
				}
				let isExists = this.$queue.isExistJd(this.footprintKey, item.skuId);
				if (isExists === false) {
					this.$queue.insert({
						key: this.footprintKey,
						value: item
					});
				}
				if (this.isEnable != '否') {
					if (grade) {
						if (grade === '0.7') {
							this.buyDes = '返现 ￥' + that.money;
						} else {
							this.buyDes = '升级返 ￥' + that.shengji;
						}
					} else {
						this.buyDes = '返现 ￥' + that.shengji;
					}
				} else {
					this.buyDes = '分享给好友';
				}
				if (!couponInfo) {
					this.getQun = '原价￥' + item.priceInfo.price + ' 购买'
				} else {
					this.getQun = '领￥' + couponInfo.discount + ' 券购买'
				}
			},

			shopCartShare: function(type) {
				let relation_id = this.$queue.getData('relation_id');
				if (type == 'fanxian') {
					if (!relation_id) {
						this.$queue.setData('href', '/pages/detail/goodsinfo?itemid=' + this.itemid);
						let token = this.$queue.getData('token');
						let userId = this.$queue.getData('userId');
						if (token) {
							uni.navigateTo({
								url: '/pages/member/publisher?uid=' + userId + '&token=' + token
							});
						} else {
							this.loginS();
						}
						return;
					}
				}
				if (type == 'fanxian') {
					if (relation_id) {
						let grade = this.$queue.getData('grade');
						if (grade == '0.7') {
							this.TklmaskShow();
						} else {
							this.shengJiMethod();
						}
					} else {
						this.getJDUrl(0)
					}
				} else {
					this.getJDUrl(0)
				}

			},
			copyTklWenAns: function() {
				uni.setClipboardData({
					data: "【京东】" + this.goods.skuName +
						'\n【在售价】' +
						this.goods.priceInfo.price +
						'元\n【券后价】' +
						this.goods.priceInfo.price - this.goods.couponInfo.couponList[0].discount +
						'元' +
						'+\n下单链接：' +
						this.erweima + '\n',
					success: r => {
						this.copyTklStatus = true;
					}
				});
			},
			getJDUrl(type) {
				let jdpid = this.$queue.getData('jdpid');
				if (!jdpid) {
					jdpid = this.$queue.getJDPid()
				}
				let url = encodeURIComponent('https://' + this.goods.materialUrl)
				let coupons = encodeURIComponent(this.goods.couponInfo.couponList[0].link)
				this.$Request.getT('/jd/goods/couponUrl?positionId=' + jdpid + '&materialId=' + url + '&couponUrl=' + coupons).then(
					res => {
						if (res.data) {
							this.erweima = res.data.shortURL;
							let url = res.data.shortURL;
							if (type == 0) {
								// #ifdef H5
								window.location.href = url
								//#endif
								// #ifdef APP-PLUS
								// if (plus.os.name == 'Android') {
								// 	url = url.split('//')[1];
								// 	plus.runtime.openURL(
								// 		url,
								// 		function(res) {
								// 			uni.navigateTo({
								// 				url: '../member/webview?url=' + res.data.shortURL
								// 			})
								// 		},
								// 		'com.jingdong.app.mall'
								// 	);
								// } else {
								// 	url = url.split('//')[1];
								// 	plus.runtime.openURL(
								// 		'openapp.jdmobile://' + url,
								// 		function(res) {
								// 			uni.navigateTo({
								// 				url: '../member/webview?url=' + res.data.shortURL
								// 			})
								// 		},
								// 		'openapp.jdmobile://'
								// 	);
								// }
								uni.navigateTo({
									url: '../member/webview?url=' + url
								})
								//#endif
							}

						} else {
							this.$queue.showToast("领券失败")
						}
					})


			},
			goPublisher() {
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
			 * 保存图片
			 * @param url
			 */
			toSave(url) {
				//#ifndef H5
				uni.getImageInfo({
					src: url,
					success: function(image) {
						console.log('图片信息：', JSON.stringify(image));
						uni.saveImageToPhotosAlbum({
							filePath: image.path,
							success: function() {
								console.log('save success');
								uni.showToast({
									title: '图片保存成功',
									icon: 'none',
									duration: 3000
								});
							}
						});
					}
				});

				//#endif
			},
			/**
			 * 返回
			 */
			goBackUp() {
				uni.switchTab({
					url: '/pages/index/index'
				});
			},
			clickCollect() {
				//收藏
				let isExist = this.$queue.isExistPdd(this.collectKey, this.itemid);
				if (isExist) {
					let items = [];
					items.push(this.itemid);
					this.$queue.removeItem(this.collectKey, items);
					this.collect.name = '收藏';
					this.collect.isCollect = '';
				} else {
					this.$queue.insert({
						key: this.collectKey,
						value: this.goods
					});
					this.collect.name = '已收藏';
					this.collect.isCollect = 'color: #FC3F78';
				}
			},

			formatDate: function(now) {
				const year = now.getFullYear();
				const month = now.getMonth() + 1;
				const date = now.getDate();
				return year + '-' + month + '-' + date;
			},

			haibao: function() {
				uni.navigateTo({
					url: '/pages/detail/share'
				});
			},
			topScrollTap: function() {
				uni.pageScrollTo({
					scrollTop: 0,
					duration: 300
				});
			},
			navBarButtontO: function(type) {
				if (type === 'search') {
					uni.switchTab({
						url: '/pages/homeSearch/index'
					});
				} else if (type === 'like') {
					uni.switchTab({
						url: '/pages/member/user'
					});
				} else if (type === 'footer') {
					uni.navigateTo({
						url: '/pages/footer/index'
					});
				} else if (type === 'home') {
					uni.switchTab({
						url: '/pages/index/index'
					});
				}
			},

			loginS() {
				//#ifdef H5
				uni.showModal({
					title: '登录提醒',
					showCancel: false,
					content: '当前账号未登录\n请登录后操作',
					success: confirmRes => {
						if (confirmRes.confirm) {
							this.$queue.setData('href', '/pages/detail/pdd?itemid=' + this.itemid);
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
					content: '当前账号未登录\n请登录后操作',
					success: confirmRes => {
						if (confirmRes.confirm) {
							this.$queue.setData('href', '/pages/detail/pdd?itemid=' + this.itemid);
							uni.navigateTo({
								url: '/pages/public/login'
							});
						}
					}
				});
				//#endif
			},
			copyTitle: function() {
				uni.setClipboardData({
					data: this.goods.title,
					success: r => {
						this.$queue.showToast('复制成功')
					}
				});
			}
		}
	};
</script>

<style scoped>
	@import '../../static/css/index.css';

	page {
		background: #F8F8F8;
	}

	.swiper-container {
		min-height: 100vw;
	}

	.hideCanvasView {
		position: relative;
	}

	.hideCanvas {
		position: fixed;
		top: -99999 upx;
		left: -99999 upx;
		z-index: -99999;
	}

	.flex_row_c_c {
		display: flex;
		flex-direction: row;
		justify-content: center;
		align-items: center;
	}

	.modalView {
		width: 100%;
		height: 100%;
		position: fixed;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		opacity: 0;
		outline: 0;
		transform: scale(3);
		perspective: 2500 upx;
		background: rgba(0, 0, 0, 0.6);
		transition: all 0.3s ease-in-out;
		pointer-events: none;
		backface-visibility: hidden;
		z-index: 999;
	}

	.modalView.show {
		opacity: 1;
		transform: scale(1);
		pointer-events: auto;
	}

	.flex_column {
		display: flex;
		flex-direction: column;
	}

	.backgroundColor-white {
		background-color: white;
	}

	.border_radius_10px {
		border-radius: 10px;
	}

	.padding1vh {
		padding: 1vh;
	}

	.posterImage {
		width: 60vw;
	}

	.flex_row {
		display: flex;
		flex-direction: row;
	}

	.marginTop2vh {
		margin-top: 2vh;
	}

	.shareInfos {
		color: #fc3f78;
	}

	.shareInfo::after {
		border: none;
	}

	button {
		font-size: 14px;
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
		width: 100px;
		height: 150px;
		position: absolute;
		right: 5%;
		top: 1%;
	}

	.b-btn {
		right: 10px;
		top: 16px;
		width: 80px;
		text-align: center;
		font-size: 14px;
		padding: 4px 1px 4px 6px;
		color: #36343c;
		border-radius: 20px;
		background: linear-gradient(left, #f9e6af, #ffd465);
		z-index: 1;
	}

	#follow {
		width: 100%;
		height: 50px;
		line-height: 50px;
		text-align: center;
		text-decoration: none;
		font-size: 18px;
		color: white;
		float: left;
		margin-top: 160px;
	}

	#follow1 {
		width: 100%;
		height: 50px;
		line-height: 50px;
		text-align: center;
		text-decoration: none;
		font-size: 18px;
		color: white;
		float: left;
		margin-top: 170px;
	}

	button::after {
		border: none;
	}

	.swiper-wrapper {
		width: 100%;
	}
</style>
