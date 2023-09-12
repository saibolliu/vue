<template>
  <div class="page-container">
    <div class="section section1">
      <h2>Section 1</h2>
      <!-- Content for section 1 -->
      <line-chart :data="lineChartData" />
    </div>
    <div class="section section2">
      <h2>Section 2</h2>
      <!-- Content for section 2 -->
      <line-chart :data="lineChartData" />
    </div>
    <div class="section section3">
      <h2>Section 3</h2>
      <!-- Content for section 3 -->
      <line-chart :data="lineChartData" />
    </div>
    <div class="section section4">
      <h2>Section 4</h2>
      <!-- Content for section 4 -->
      <line-chart :data="lineChartData" />
    </div>
  </div>
</template>

<script>
import LineChart from './LineChart.vue'; // 路径根据实际情况修改
export default {
  components: {
    LineChart
  },
  data() {
    return {
      lineChartData: [820, 932, 901, 934, 1290, 1330, 1320] // 折线图数据
    };
  }
  // Component logic and behavior
  // Component logic and behavior
}
</script>

<style scoped>
.page-container {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-template-rows: repeat(2, 1fr);
  gap: 20px;
  width: 100%;
  height: 100vh;
  padding: 20px;
}

.section {
  border: 1px solid #ccc;
  padding: 20px;
  text-align: center;
}

.section1 { grid-area: 1 / 1 / 2 / 2; }
.section2 { grid-area: 1 / 2 / 2 / 3; }
.section3 { grid-area: 2 / 1 / 3 / 2; }
.section4 { grid-area: 2 / 2 / 3 / 3; }
</style>
