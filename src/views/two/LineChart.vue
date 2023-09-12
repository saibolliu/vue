<template>
    <div class="line-chart">
      <div ref="chart" class="chart-container"></div>
    </div>
  </template>
  
  <script>
  import * as echarts from 'echarts';
  
  export default {
    props: {
      data: {
        type: Array,
        required: true
      }
    },
    mounted() {
      this.drawLineChart();
    },
    methods: {
      drawLineChart() {
        const chartDom = this.$refs.chart;
        const chart = echarts.init(chartDom);
  
        const option = {
          title: {
            text: '折线图示例'
          },
          xAxis: {
            type: 'category',
            data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
          },
          yAxis: {
            type: 'value'
          },
          series: [{
            data: this.data,
            type: 'line'
          }]
        };
  
        chart.setOption(option);
      }
    }
  }
  </script>
  
  <style scoped>
  .chart-container {
    width: 100%;
    height: 300px;
  }
  </style>