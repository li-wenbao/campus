<template>
	<view class="page">
		<scroll-view scroll-y :style="style" @scrolltolower="toLowFun">
			<view v-if="orderList.length>0">
				<view class="margin-sm radius padding-sm bg-white radius20" v-for="(item,index) in orderList" :key="index">
					<view class="flex">
						<view class="margin-right-sm" style="width:80rpx;height:80rpx; overflow: hidden;" :data-id="item.id" @click="_goToStore">
							<image style="width:80rpx;height:80rpx;border-radius:4rpx;" mode="scaleToFill" lazy-load="true" :src="item.imgUrl" />
						</view>
						<view style="width:570rpx">
							<view class="flex align-center padding-bottom-xs" style="height:80rpx">
								<view class="flex-treble flex align-center" style="width:380rpx;" :data-id="item.id" @click="_goToStore">
									<text class="text-black text-bold text-xl text-cut">{{item.name}}</text>
									<text class="cuIcon-right padding-left-sm"></text>
								</view>
								<view class="flex-sub text-right" :data-id="item.orderId" @click="_goDetails">
									<text>{{item.status}}</text>
								</view>
							</view>
							<view class="margin-top-xs padding-top-xs solid-top" @click="_goDetails">
								<view class="flex justify-between text-gray" v-for="(listItem,listIndex) in item.list" :key="listIndex">
									<view>{{listItem.name}}</view>
									<view>x{{listItem.count}}</view>
								</view>
								<view class="margin-top-xs text-right">
									共{{item.amount}}件商品，实付
									<text class="text-price text-orange">{{item.sum}}</text>
								</view>
							</view>
						</view>
					</view>
					<view class="margin-top flex justify-end text-center">
						<button class="cu-btn line-gray text-black radius20" role="button" aria-disabled="false" :data-id="item.orderId"
						 @click="_goEvaluate">去评价</button>
					</view>
				</view>
				<view v-if="loading" class="cu-load loading"></view>
				<view v-if="over" class="cu-load over"></view>
			</view>
			<view v-else>暂无订单,一大波美食正在袭来~</view>
		</scroll-view>
	</view>
</template>

<script>
	// 引入登录模态弹窗组件
	var {
		log
	} = console
	export default {

		props: {
			orderList: Array,
			busidata: Object,
			loading: {
				type: Boolean,
				default: false
			},
			over: {
				type: Boolean,
				default: false
			}
		},
		data() {
			return {

			}
		},

		methods: {

		},

		computed: {
			style() {
				let CustomBar = this.CustomBar;
				return `height:calc(100vh - ${CustomBar}px - 45px);`;
			}
		},

		watch: {

		}
	}
</script>

<style scoped>
	.ordering-fls {
		display: flex;
		justify-content: space-between;
	}

	/* 左边商品分类 */
	.ordering-left {
		width: 190upx;
		background: #fafafa;
		overflow-y: auto;
		height: 100%;
	}

	.ordering-left text {
		color: #a8a8a8;
		display: block;
		font-size: 28upx;
		height: 80upx;
		line-height: 80upx;
		text-align: center;
	}

	.activeord {
		background: #FFFFFF !important;
		font-weight: bold !important;
	}

	/* 右边 */
	.ordering-right {
		width: 100%;
		padding: 9upx 9upx 130upx 9upx;
		overflow: hidden;
		overflow-y: auto;
	}

	.activeord {
		background: #FFFFFF !important;
		font-weight: bold !important;
	}

	.ordering-right-title {
		font-size: 28upx;
		height: 60upx;
		line-height: 60upx;
	}

	.content-view {
		display: flex;
		justify-content: space-between;
		height: 200upx !important;
		overflow: hidden;
		margin: 30upx 0;
		color: #898989;
	}

	.content-img {
		width: 180rpx !important;
		height: 200rpx !important;
		overflow: hidden;
	}

	.content-img image {
		width: 100%;
		height: 100%;
		border-radius: 10upx;
		overflow: hidden;
	}

	.content-title {
		/* width: 100%; */
		padding-left: 10upx;
		font-size: 24upx;
	}

	.conteng-take {
		color: #333333;
		height: 50upx;
		font-size: 33upx;
		font-weight: bold;
		line-height: 50upx;
		display: -webkit-box;
		-webkit-box-orient: vertical;
		-webkit-line-clamp: 1;
		overflow: hidden;
	}

	.conteng-monthly {
		display: flex;
	}

	.conteng-monthly text {
		background: #f1f1f1;
		margin-right: 9upx;
		padding: 4upx 13upx;
		border-radius: 7upx;
		display: block;
	}

	.conteng-starting {
		display: flex;
		align-items: center;
		height: 50upx;
		line-height: 50upx;
	}

	.conteng-price image {
		width: 60upx;
		height: 60upx;
	}

	.conteng-price {
		display: flex;
		align-items: center;
	}

	.ordering-price {
		display: flex;
		align-items: center;
		justify-content: space-around;
		width: 100%;
	}

	/* 数量 */
	.amounting {
		width: 50upx;
		text-align: center;
	}

	.conteng-hua {
		text-decoration: line-through;
	}

	.conteng-shi {
		color: #fb4e44;
		font-size: 33upx;
		width: 180upx;
		overflow: hidden;
	}

	/* 购物车 */
	.Shopping {
		position: fixed;
		left: 0;
		right: 0;
		bottom: 0;
		width: 100%;
	}

	.Shopping-left {
		height: 130upx;
		background: #000000;
		flex-grow: 2;
		border-top-left-radius: 50upx;
		border-bottom-left-radius: 50upx;
	}

	.Shopping-right {
		height: 130upx;
		line-height: 130upx;
		font-size: 30upx;
		color: #7f7f7f;
		text-align: center;
		background: #000000;
		flex-grow: 1;
		border-top-right-radius: 50upx;
		border-bottom-right-radius: 50upx;
	}

	.Shopping-dis {
		display: flex;
		height: 130upx;
		margin: 0 20upx;
	}

	.qishou {
		width: 120upx;
		height: 150upx;
		position: fixed;
		bottom: 0upx;
		left: 50upx;
	}

	.qishou image {
		width: 120upx;
		height: 150upx !important;
	}

	/* 定位 */
	.Delivery {
		color: #999999;
		font-size: 28upx;
		height: 65upx;
	}

	.Shopping-left text {
		display: block;
	}

	.Total-price {
		font-size: 40upx;
		color: #FFFFFF;
		height: 65upx;
		line-height: 65upx;
	}

	.Cost-mony {
		padding-left: 170upx;
		color: #4CD964;
		height: 130upx;
	}

	/* 数量 */
	.Numbering {
		background: red;
		width: 40upx;
		height: 40upx;
		border-radius: 50%;
		font-size: 20upx;
		color: #FFFFFF;
		text-align: center;
		line-height: 40upx;
		position: fixed;
		bottom: 50upx;
		left: 130upx;
	}

	/* 选中菜品 */
	.shoppactive {
		background: #ffd300 !important;
		color: #000000 !important;
	}
</style>
