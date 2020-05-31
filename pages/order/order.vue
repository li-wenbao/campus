<template>
	<view class="page">
		<cu-custom :bgColor="'bg-blue'" :isBack="false">
			<block slot="backText"></block>
			<block slot="content">
				<!-- #ifndef MP-ALIPAY -->
				订单
				<!-- #endif -->
			</block>
			<block slot="right">
				<view class="action"></view>
			</block>
		</cu-custom>
		<!-- tab切换 -->
		<Tabs></Tabs>
		<TabsList :orderList="orderList" style="height: ;"
		:style="'height:100vh -'+ CustomBar + 'px;'"
		></TabsList>
	</view>
</template>

<script>
	let {
		log
	} = console
	import Json from '../../json.js';
	import Tabs from "./components/tabs.vue"
	import TabsList from "./components/tabsList.vue"
	export default {
		components: {
			Tabs,
			TabsList
		},
		data() {
			return {
				CustomBar: this.CustomBar,
				tabList: ["全部订单", "待评价", "退款"],
				tabCur: 0,
				top: 0,
				loading: false,
				over: false,
				count: 0,
				// orderList:[]
			}
		},
		onLoad() {
			console.log("this.CustomBar==", this.CustomBar)
			this.getOrderList()
		},

		computed: {
			style() {
				let CustomBar = this.CustomBar;
				return `height:calc(100vh - ${CustomBar}px - 45px);`;
			}
		},
		methods: {

			// 广告轮播图部分 列表数据
			getOrderList: function() {
				this.orderList = Json.orderList
				// log(this.swiperList)
			},
			/*
			 *加载更多
			 */
			toLowFun() {
				console.log("触底事件");
				if (!this.over) {
					if (this.count < 5) {
						this.loading = true;
						var _this = this;
						setTimeout(function() {
							let data = _this.orderList;
							_this.orderList.push(data[0], data[1]);
							_this.loading = false;
							_this.count++;
						}, 2000);
					} else {
						this.over = true;
					}
				} else {
					this.loading = false;
				}
			},
			//跳转至商家店铺
			_goToStore(e) {
				console.log(e);
				uni.navigateTo({
					url: "/pages/home/storetest?id=" + e.currentTarget.dataset.id
				});
			},
			//查看订单详情
			_goDetails(e) {
				uni.navigateTo({
					url: "/pages/order/details?orderId=" + e.currentTarget.dataset.id
				});
			},
			//去评价
			_goEvaluate(e) {
				console.log(e)
				uni.navigateTo({
					url: "/pages/order/evaluate?orderId=" + e.currentTarget.dataset.id
				});
			},
			//查看退款进度
			_goRefund(e) {
				uni.navigateTo({
					url: "/pages/order/refund?orderId=" + e.currentTarget.dataset.id
				});
			},

			//才来一单
			_goSetaList(e) {
				uni.navigateTo({
					url: "/pages/home/storetest?orderId=" + e.currentTarget.dataset.id
				});
			}
		},
	}
</script>

<style>

</style>
