<template>
  <v-container>
    <v-row>
      <v-col cols="12" md="4">
        <v-select
          v-model="category"
          :items="categoryOptions"
          label="Category"
        />
      </v-col>

      <v-col cols="12" md="4">
        <v-slider
          v-model="value"
          min="0"
          max="100"
          step="10"
          label="Value"
        />
      </v-col>

      <v-col cols="12" md="4">
        <v-select
          v-model="dataType"
          :items="chartTypes"
          label="Chart Type"
        />
      </v-col>

      <v-col cols="12" md="4">
        <v-text-field
          v-model="numItems"
          label="Number of Items"
          type="number"
        />
      </v-col>
    </v-row>

    <div id="chart" style="width: 100%; height: 400px;"></div>
  </v-container>
</template>

<script>
import * as echarts from 'echarts';

export default {
  data() {
    return {
      // Data bound to the form inputs
      category: 'Category A',
      value: 50,
      dataType: 'Line',
      numItems: 5,

      // Options for dropdowns and controls
      categoryOptions: ['Category A', 'Category B', 'Category C'],
      chartTypes: ['Line', 'Bar', 'Pie'],

      // Chart instance and data
      chartInstance: null,
      chartData: []
    };
  },
  methods: {
    // Generate random data based on user input
    generateData() {
      this.chartData = Array.from({ length: this.numItems }, (_, i) => ({
        name: `${this.category} ${i + 1}`,
        value: Math.round(Math.random() * this.value),
      }));
    },
    // Update the ECharts chart
    updateChart() {
      if (this.chartInstance) {
        const option = {
          title: { text: 'Dynamic Chart' },
          tooltip: {},
          xAxis: { type: 'category', data: this.chartData.map((item) => item.name) },
          yAxis: { type: 'value' },
          series: [
            {
              type: this.dataType.toLowerCase(),
              data: this.chartData.map((item) => item.value),
            },
          ],
        };
        this.chartInstance.setOption(option);
      }
    }
  },
  watch: {
    // Watchers for input changes to trigger data and chart updates
    category() { this.generateData(); },
    value() { this.generateData(); },
    dataType() { this.updateChart(); },
    numItems() { this.generateData(); },
    chartData() { this.updateChart(); }
  },
  mounted() {
    // Initialize ECharts when component is mounted
    this.chartInstance = echarts.init(document.getElementById('chart'));
    this.generateData();
  }
};
</script>

<style scoped>
#chart {
  margin-top: 20px;
}
</style>