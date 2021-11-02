<template>
  <PieChart ref="chart" />
</template>

<script>
import { Doughnut as PieChart } from "vue-chartjs";

import { ARRAY_PROP_DEFAULT } from "@/utils/consts";

export default {
  props: {
    legend: {
      type: Array,
      default: ARRAY_PROP_DEFAULT,
    },
  },
  components: {
    PieChart,
  },
  mounted() {
    this.makeChart();
  },
  methods: {
    makeChart() {
      const legendChartData = {
        labels: this.legend.map((it) => it.text),
        datasets: [
          {
            label: "Легенда",
            backgroundColor: this.legend.map((legendItem) => legendItem.color),
            data: this.legend.map((legendItem) => legendItem.counter),
          },
        ],
      };

      const options = {
        borderWidth: "10px",
        legend: {
          display: false,
        },
      };

      this.$refs.chart.renderChart(legendChartData, options);
    },
  },
};
</script>
