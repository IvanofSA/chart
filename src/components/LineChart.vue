<script>
	import {Line} from "vue-chartjs";

	export default {
		extends: Line,
		props: ["data", "options", "labes", "name"],
		mounted() {
			this.renderLineChart();
		},
		computed: {
			chartData() {
				return this.data;
			},
			labesData() {
				return this.labes;
			}
		},
		methods: {
			renderLineChart() {
				this.renderChart(
					{
						labels: this.labesData,
						datasets: [
							{
								label: this.name,
								data: this.chartData,
								backgroundColor: "transparent",
								borderColor: "rgba(1, 116, 188, 0.50)",
							}
						]
					},
					{
						responsive: true,
						maintainAspectRatio: false,
					}
				);
			}
		},
		watch: {
			data: function () {
				this.renderLineChart();
			}
		}
	};
</script>