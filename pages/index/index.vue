<template>
	<view class="content">
		<image :src="img"></image>
		<canvas class="canvasBg" id="myCanvas" canvas-id="myCanvas"></canvas>
		<view class="operation">
			<view class="uni-form-item uni-column">
				<view class="title">背景图片地址</view>
				<input class="uni-input" v-model="img" placeholder="图片地址" />
			</view>
			<button type="warn" class="btn" @click="empty">清空所有色块</button>
			<br>
			<view class="uni-form-item uni-column" v-for="(item,index) of dataVal" :key="index">
				<view class="title">色块{{index+1}}</view>
				<input class="uni-input" v-model="item.color" placeholder="颜色值" />
				<view v-for="(i,k) of item.list" :key="k">
					<view style="display: flex;align-items:center;padding: 0 50rpx;">
						x:<input v-model="i.x" placeholder="x坐标" @blur="blurInput" />
						y:<input v-model="i.y" placeholder="y坐标" @blur="blurInput" />
					</view>
				</view>
				<button type="primary" class="btn" @click="addMap(item)">新增坐标</button>
			</view>
			<view style="display: flex;justify-content: center;">
				--------------------------------------
			</view>
			<button type="warn" class="btn" @click="add">新增色块</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				bg: 'rgba(229,229,229,0.7)',
				sel: 'rgba(0,0,0,0.3)',
				img: 'https://cdgwin-test.cn-bj.ufileos.com/gwin-platform/gwin-cloud/46f973f6b9cf42dd80c853ec104124e8.png',
				dataVal: [{
						list: [{
								x: 218,
								y: 318
							},
							{
								x: 277,
								y: 363
							},
							{
								x: 277,
								y: 383
							},
							{
								x: 195,
								y: 425
							},
							{
								x: 128,
								y: 379
							},
							{
								x: 128,
								y: 364
							}
						],
						status: 1,
						color: 'rgba(0,0,0,0.3)'
					},
					{
						list: [{
								x: 59,
								y: 309
							},
							{
								x: 226,
								y: 227
							},
							{
								x: 358,
								y: 321
							},
							{
								x: 358,
								y: 341
							},
							{
								x: 276,
								y: 382
							},
							{
								x: 274,
								y: 362
							},
							{
								x: 218,
								y: 319
							},
							{
								x: 128,
								y: 366
							},
							{
								x: 128,
								y: 378
							},
							{
								x: 59,
								y: 329
							}
						],
						status: 1,
						color: 'rgba(229,229,229,0.7)'
					}
				],
			}
		},
		onLoad() {
			this.ctx = uni.createCanvasContext('myCanvas')


			// 绘制图片 初始化给个默认给个数据   
			this.drawImg(this.dataVal)
		},
		methods: {
			// 改变数据后 渲染
			blurInput() {
				this.drawImg(this.dataVal)
			},
			// 新增坐标
			addMap(item) {
				item.list.push({
					x: '',
					y: ''
				})
			},
			// 清空
			empty() {
				this.dataVal = [{
					list: [{
						x: '',
						y: ''
					}],
					status: 1,
					color: 'rgba(229,229,229,0.7)'
				}]
				this.drawImg(this.dataVal)
			},
			// 新增色块
			add() {
				const item = {
					list: [],
					status: 1,
					color: 'rgba(0,0,0,0.3)'
				}
				this.dataVal.push(item)
				this.drawImg(this.dataVal)

			},
			// 绘制图片坐标
			drawImg(val) {
				val.forEach((key) => {
					if (key.status == 1) {
						// 路径移动到指定点   线条颜色
						this.ctx.beginPath()
						this.ctx.setStrokeStyle('#ffffff')
						// 新起点
						key.list.forEach((item, index) => {
							if (index == 0) {
								this.ctx.moveTo(item.x, item.y)
							} else {
								this.ctx.lineTo(item.x, item.y)
							}
						})
						// 关闭路径
						this.ctx.closePath()
						// 画线条
						this.ctx.stroke()
						// 背景颜色
						this.ctx.fillStyle = key.color
						this.ctx.fill()
					}
				})

				this.ctx.draw()
			},
		},
	}
</script>

<style lang="scss">
	.content {
		position: fixed;
		width: 100%;

		.operation {
			overflow-y: scroll;
			height: calc(100vh - 920rpx);

		}

		input {
			border: 1rpx solid #000000;
			margin: 10rpx 50rpx;
			font-size: 30rpx;
			padding: 10rpx;
			border-radius: 10rpx;
		}

		.btn {
			margin: 50rpx;
			margin-bottom: 150rpx;
		}

		image {
			width: 100%;
			height: 936rpx;
		}

		.canvasBg {
			display: inline-block !important;
			position: fixed;
			left: 0;
			top: 0;
			width: 100%;
			height: 935rpx;
			z-index: 99999;
		}
	}
</style>
