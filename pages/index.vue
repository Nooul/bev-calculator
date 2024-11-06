<template>
  <v-container>
    <!-- Input Controls -->
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

    <!-- Chart Component -->
    <InteractiveChart :data="chartData" :type="dataType" />
  </v-container>
</template>

<script>
import InteractiveChart from '~/components/InteractiveChart.vue';

export default {
  components: {
    InteractiveChart
  },
  data() {
    return {
      // Data for input controls
      category: 'Category A',
      value: 50,
      dataType: 'Line',
      numItems: 5,

      // Options for controls
      categoryOptions: ['Category A', 'Category B', 'Category C'],
      chartTypes: ['Line', 'Bar', 'Pie'],

      // Chart data
      chartData: []
    };
  },
  methods: {
    // Generate data based on inputs
    generateData() {
      this.chartData = Array.from({ length: this.numItems }, (_, i) => ({
        name: `${this.category} ${i + 1}`,
        value: Math.round(Math.random() * this.value)
      }));
    }
  },
  watch: {
    // Watchers to update chart data when inputs change
    category() { this.generateData(); },
    value() { this.generateData(); },
    numItems() { this.generateData(); }
  },
  mounted() {
    // Generate initial data when component mounts
    this.generateData();
  }
};
</script>