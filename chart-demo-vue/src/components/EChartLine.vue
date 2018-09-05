<template>
  <div class="chart" :id="id"></div>
</template>
<script>
/* eslint-disable */

export default {
  props: {
    id: {
      type: String,
      required: true
    },
    rawData: {
      type: Array,
      required: true
    }
  },
  mounted() {
    this.renderEChart()
  },
  methods: {
    renderEChart() {
      const element = document.getElementById(this.id)
      this.myChart = echarts.init(element)

      var option = {
        legend: {
          bottom: 0
        },
        tooltip: {},
        dataset: {
          source: this.rawData
        },
        xAxis: { type: 'category' },
        yAxis: {
          axisLabel: {
            formatter: value => '$' + parseFloat(value).toLocaleString()
          }
        },
        series: ['', '', ''].map(() => ({
          type: 'line',
          barGap: 0,
          label: {
            normal: {
              position: 'top',
              show: true,
              formatter: data =>
                '$' +
                parseFloat(data.value[data.seriesIndex + 1]).toLocaleString()
            }
          }
        }))
      }

      this.myChart.setOption(option)
    },
    resizeChart() {
      if (this.myChart && this.myChart.resize) {
        this.myChart.resize()
      }
    },
    transposeArray(arr) {
      const newArray = []
      for (let i = 0; i < arr.length; i++) {
        for (let j = 0; j < arr[i].length; j++) {
          if (!newArray[j]) {
            newArray[j] = []
          }

          newArray[j][i] = arr[i][j]
        }
      }

      return newArray
    }
  }
}
</script>
<style>
.chart {
  height: 500px;
  background: white;
  padding: 20px;
  margin: 20px 0;
  border: solid 1px rgb(232, 234, 236);
}
</style>

