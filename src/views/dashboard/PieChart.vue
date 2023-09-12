<!-- PieChart.vue -->
<template>
    <div class="pie-chart">
      <div ref="chart" class="chart-container"></div>
    </div>
  </template>
  
  <script>
  import * as echarts from 'echarts';
  
  export default {
    props: {
      pieData: {
        type: Array,
        required: true
      }
    },
    mounted() {
      console.log('Received pieData:', JSON.stringify(this.pieData));
      this.$nextTick(() => {
        this.initChart();
      });
    },
    methods: {
  initChart() {
    const chartDom = this.$refs.chart;
    const chart = echarts.init(chartDom);

    const option = {
      title: {
        text: '项目签约图例',
        top: '5%', 
      },
      tooltip: {
        trigger: 'item',
        formatter: '{a} <br/>{b}: {c} ({d}%)'
      },
      legend: {
        top: '5%',
    left: 'center',
        data: this.pieData.map(item => item.name)
      },
      
      series: [
        {
          name: '',
          type: 'pie',
          radius: ['50%', '70%'],
          avoidLabelOverlap: false,
          label: {
            show: false,
            position: 'center'
          },
          emphasis: {
            label: {
              show: true,
              fontSize: '30',
              fontWeight: 'bold'
            }
          },
          labelLine: {
            show: false
          },
          data: this.pieData
        }
      ]
    };

    chart.setOption(option);
  }
}
  };
  </script>
  
  <style scoped>
  .chart-container {
    width: 100%;
    height: 400px;
  }
  </style>
  