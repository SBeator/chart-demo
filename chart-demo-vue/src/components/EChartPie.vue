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
        tooltip: {},
        dataset: {
          source: this.titleToString(this.transposeArray(this.rawData))
        },
        series: [
          {
            type: 'pie',
            radius: 60,
            name: '2001',
            label: {
              normal: {
                show: true,
                formatter: '{b}: {d}%'
              }
            },
            center: ['30%', '30%'],
            // No encode specified, by default, it is '2012'.
            encode: {
              itemName: 'Year',
              value: '2001'
            }
          },
          {
            type: 'pie',
            radius: 60,
            name: '2006',
            label: {
              normal: {
                show: true,
                formatter: '{b}: {d}%'
              }
            },
            label: {
              normal: {
                show: true,
                formatter: '{b}: {d}%'
              }
            },
            center: ['70%', '30%'],
            encode: {
              itemName: 'Year',
              value: '2006'
            }
          },
          {
            type: 'pie',
            radius: 60,
            name: '2011',
            label: {
              normal: {
                show: true,
                formatter: '{b}: {d}%'
              }
            },
            label: {
              normal: {
                show: true,
                formatter: '{b}: {d}%'
              }
            },
            center: ['30%', '70%'],
            encode: {
              itemName: 'Year',
              value: '2011'
            }
          }
        ]
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

      console.log(newArray)
      return newArray
    },
    titleToString(arr) {
      arr[0] = arr[0].map(data => data + '')
      return arr
    }
  }
}
</script>
<style scoped>
.chart {
  height: 500px;
  background: white;
  padding: 20px;
  margin: 20px 0;
  border: solid 1px rgb(232, 234, 236);
}
</style>

