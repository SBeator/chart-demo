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
        tooltip: {},
        legend: {
          data: this.rawData[0].slice(1),
          bottom: 0
        },
        dataset: {
          source: this.titleToString(this.transposeArray(this.rawData))
        },

        // dataset: {
        //   source: [
        //     ['Year', '2001', '2006', '2011'],
        //     ['median	gross	annual	household	income', 40768, 40768, 40768],
        //     ['median	annual	rent', 7540, 7540, 7540],
        //     ['median	annual	mortgage	payments', 10404, 10404, 10404]
        //   ]
        // },

        // dataset: {
        //   source: [
        //     ['product', '2012', '2013', '2014', '2015', '2016', '2017'],
        //     ['Matcha Latte', 41.1, 30.4, 65.1, 53.3, 83.8, 98.7],
        //     ['Milk Tea', 86.5, 92.1, 85.7, 83.1, 73.4, 55.1],
        //     ['Cheese Cocoa', 24.1, 67.2, 79.5, 86.4, 65.2, 82.5],
        //     ['Walnut Brownie', 55.2, 67.1, 69.2, 72.4, 53.9, 39.1]
        //   ]
        // },
        series: [
          {
            type: 'pie',
            radius: 60,
            center: ['25%', '30%'],
            // No encode specified, by default, it is '2012'.
            encode: {
              itemName: 'Year',
              value: '2006'
            }
          },
          {
            type: 'pie',
            radius: 60,
            center: ['75%', '30%'],
            encode: {
              itemName: 'Year',
              value: '2006'
            }
          },
          {
            type: 'pie',
            radius: 60,
            center: ['25%', '70%'],
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
  height: 600px;
  background: white;
  padding: 20px;
  margin: 20px 0;
  border: solid 1px rgb(232, 234, 236);
}
</style>

