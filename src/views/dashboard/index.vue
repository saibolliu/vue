<template>
  <div class="dashboard-container">
    <PieChart v-if="chartData.length > 0" :pieData="chartData" />
    <ButtonLink />
    <DataDisplay />
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
import axios from 'axios'
import PieChart from './PieChart.vue'
import ButtonLink from './ButtonLink.vue'
import DataDisplay from './DataDisplay.vue'

export default {
  name: 'Dashboard',
  components: {
    PieChart,
    ButtonLink,
    DataDisplay
  },
  data() {
    return {
      chartData: []
    }
  },
  computed: {
    ...mapGetters([
      'name'
    ])
  },
  mounted() {
    // Simulate API call
    axios.post('/api/zhuzhuangtu', {
      username: 'admin',
      password: 'admin'
    })
    .then(response => {
      this.chartData = response.data;
    })
    .catch(error => {
      console.error('API error:', error);
    });
  }
}
</script>

<style lang="scss" scoped>
.dashboard {
  &-container {
    margin: 30px;
  }
  &-text {
    font-size: 30px;
    line-height: 46px;
  }
}
</style>