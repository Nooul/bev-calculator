<template>
  <div id="chart" style="width: 100%; height: 400px;"></div>
</template>

<script>
import * as echarts from 'echarts';

export default {
  props: {
    data: {
      type: Array,
      required: true
    },
    type: {
      type: String,
      default: 'line'
    }
  },
  data() {
    return {
      chartInstance: null
    };
  },
  watch: {
    // Watch for data or type changes to update the chart
    data: 'updateChart',
    type: 'updateChart'
  },
  methods: {
    updateChart() {
      if (this.chartInstance) {
        const option = {
          title: { text: 'Dynamic Chart' },
          tooltip: {},
          xAxis: { type: 'category', data: this.data.map((item) => item.name) },
          yAxis: { type: 'value' },
          series: [
            {
              type: this.type.toLowerCase(),
              data: this.data.map((item) => item.value)
            }
          ]
        };
        this.chartInstance.setOption(option);
      }
    }
  },
  mounted() {
    // Initialize ECharts when component mounts
    this.chartInstance = echarts.init(this.$el);
    this.updateChart();
  }
};
</script>

<style scoped>
#chart {
  margin-top: 20px;
}
</style>