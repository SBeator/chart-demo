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
    },
    title: {
      type: String,
      required: true
    },
    subTitle: {
      type: String,
      default: ''
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
        title: {
          text: this.title,
          subtext: this.subTitle
        },
        legend: {
          bottom: 0
        },
        tooltip: {
          formatter: data =>
            console.log(data) ||
            data.seriesName +
              ':' +
              parseFloat(data.data[data.seriesIndex + 1] * 100).toFixed(2) +
              '%'
        },
        dataset: {
          source: this.rawData
        },
        xAxis: { type: 'category' },
        yAxis: {
          min: function(value) {
            return Math.max(0, value.min - 0.1)
          },
          max: function(value) {
            return value.max + 0.05
          },
          axisLabel: {
            formatter: value => parseFloat(value * 100).toFixed(0) + '%'
          }
        },
        series: Array(this.rawData[0].length - 1)
          .fill(1)
          .map(() => ({
            type: 'line',
            barGap: 0,
            label: {
              normal: {
                position: 'top',
                show: true,
                formatter: data =>
                  parseFloat(data.value[data.seriesIndex + 1] * 100).toFixed(
                    2
                  ) + '%'
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
  height: 400px;
  background: white;
  padding: 20px;
  margin: 20px 0;
  border: solid 1px rgb(232, 234, 236);
}
</style>

