<template>
  <div>
    <div class="chart" :id="id"></div>
    <!-- <div class="legend">

    </div> -->
  </div>

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
          bottom: 0,
          data: ['median annual rent', 'median annual mortgage payments']
        },
        tooltip: {},
        dataset: {
          source: this.titleToString(this.transposeArray(this.rawData))
        },
        series: [
          {
            type: 'pie',
            radius: 60,
            startAngle: 120,
            name: '2001',
            label: {
              normal: {
                show: true,
                formatter: '{b}: {d}%'
              }
            },
            center: ['20%', '50%'],
            // No encode specified, by default, it is '2012'.
            encode: {
              itemName: 'Year',
              value: '2001'
            }
          },
          {
            type: 'pie',
            startAngle: 120,
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
            center: ['45%', '50%'],
            encode: {
              itemName: 'Year',
              value: '2006'
            }
          },
          {
            type: 'pie',
            startAngle: 120,
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
            center: ['70%', '50%'],
            encode: {
              itemName: 'Year',
              value: '2011'
            }
          }
        ]
      }

      this.myChart.setOption(option)

      const map = {
        'median annual rent': 'income left',
        'median annual mortgage payments': 'Income left'
      }
      const left = name => console.log(map[name]) || map[name]

      this.myChart.dispatchAction({
        type: 'legendUnSelect',
        name: 'median annual rent'
      })

      this.myChart.dispatchAction({
        type: 'legendUnSelect',
        name: left('median annual rent')
      })

      this.myChart.on('legendselectchanged', obj => {
        console.log(obj)
        const { selected, name } = obj

        // // 使用 legendToggleSelect Action 会重新触发 legendselectchanged Event，导致本函数重复运行
        // // 使得 无 selected 对象
        if (selected != undefined) {
          const otherEventType = selected[name]
            ? 'legendUnSelect'
            : 'legendSelect'

          this.rawData[0].slice(1).forEach(otherName => {
            console.log(otherName)
            if (otherName != name && otherName != left(name)) {
              console.log(otherEventType)
              console.log(otherName)

              this.myChart.dispatchAction({
                type: otherEventType,
                name: otherName
              })
            }
          })

          this.myChart.dispatchAction({
            type: selected[name] ? 'legendSelect' : 'legendUnSelect',
            name: left(name)
          })
        }
      })
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
  height: 350px;
  background: white;
  padding: 20px;
  margin: 20px 0;
  border: solid 1px rgb(232, 234, 236);
}
</style>

