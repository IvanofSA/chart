<template>
	<div class="container">
		<div class="table">
			<div class="table__main" v-for="(row, i) in list" :key="i">
				<div class="table__row table__row_head" @click="row.open = !row.open">
					<div class="table__cell">{{row.name}}</div>
					<div class="table__cell" data-id="sum">{{row.sum[i].price}}</div>
					<div class="table__cell" data-id="sum">{{row.sum[i].price}}</div>
				</div>

				<div class="table__drop" v-if="row.open">
					<div class="table__chart">
						<LineChart :name="row.name" :labes="getLabels(row.list)"
								   :data="getDataChart(row.list)"></LineChart>
					</div>
					<div class="table__row" v-for="(item, j) in row.list" :key="j" @click="filterData(item, item.name)">
						<div class="table__cell">{{item.name}}</div>
						<div class="table__cell" :data-date="val.date" v-for="(val, k) in item.list" :key="k">
							{{val.price}}
						</div>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
	import axios from 'axios';
	import LineChart from "./LineChart.vue";


	export default {
		name: 'index',
		components: {
			LineChart
		},
		data() {
			return {
				list: [],
				open: false
			}
		},
		created() {
			axios.get('static/data.json').then((response) => {
				this.list = response.data;
				for (let i in this.list) {
					this.$set(this.list[i], 'open', false)
				}
				this.getData();
			}).catch((error) => {
				console.log(error)
			})
		},
		methods: {
			filterData(arr, filter) {
				for (let item in this.list) {
					for (let j in this.list[item].list) {
						if (this.list[item].list[j].name === filter) {
							this.list[item].list.splice(j, 1)
						}
					}
				}
			},
			getLabels(arr) {
				let labelsArr = [];
				for (let i in arr) {
					labelsArr.push(arr[i].name)
				}
				return labelsArr
			},
			getDataChart(arr) {
				let today = [],
					yesterday = [];

				for (let i in arr) {
					for (let j in arr[i].list) {
						if (arr[i].list[j].date === 'today') {
							today.push(arr[i].list[j].price)
						}
						if (arr[i].list[j].date === 'yesterday') {
							yesterday.push(arr[i].list[j].price)
						}
					}
				}
				let diff = today.map((el, i) => {
					return el - yesterday[i];
				});
				return diff
			}
		}
	}
</script>

<style scoped lang="scss">
	* {
		box-sizing: border-box;
	}
	.container {
		display: flex;
		align-items: center;
		justify-content: center;
		min-width: 400px;
		margin: 0 auto;
		position: relative;


		@media (min-width: 500px) {
			max-width: 800px;
			margin: 0 auto;
			padding: 0 20px;
		}
	}
	.table {
		background: #f5f5f5;

		&__row {
			display: flex;
			flex-flow: row wrap;
			align-items: flex-start;
			justify-content: space-between;
			width: 100%;
			min-width: 400px;
			background: #e1e1e1;
			margin-bottom: 2px;

			&_head {
				background: #d7d7d7;
			}

			&:hover {
				background: #ccc;
			}
		}

		&__main {
			cursor: pointer;
		}

		&__cell {
			width: 33%;
			height: 30px;
			line-height: 28px;
			border-right: 1px solid #fff;
			text-align: center;

			&:last-child {
				border-right: 0;
			}
		}

		&__chart {
			display: block;
			min-width: 320px;
			width: 100% !important;

			canvas {
				width: 100% !important;
			}
		}

		#line-chart {
			width: 100% !important;
		}
	}
</style>
