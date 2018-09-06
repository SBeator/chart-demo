<template>
  <div>
    <Row>
      <br/>
    </Row>
    <Row>
      <Col span="20" offset="2" class="relative">
      <Table :columns="tableColumns" :data="tableData"></Table>
      </Col>
    </Row>
    <Row v-if="showOrigin">
      <Col span="20" offset="2">
      <Button class="view" @click="switchView" type="primary">View the progress</Button>
      <EChartBar id="chart_1" :rawData="rawData" title="Housing affortablity"></EChartBar>
      </Col>
    </Row>
    <Row v-if="!showOrigin">
      <Col span="20" offset="2">
      <Button class="view" @click="switchView" type="primary">View the origin chart</Button>
      <EChartLine id="chart_2" :rawData="rawData" title="Housing affortablity"></EChartLine>
      </Col>
    </Row>
    <Row v-if="!showOrigin">
      <Col span="20" offset="2">
      <EChartPie id="chart_3" :rawData="pieData" title="Housing affortablity" subTitle="Other payment = median gross annual household income - median annual rent - median annual mortgage payments"></EChartPie>
      </Col>
    </Row>
    <Row v-if="!showOrigin">
      <Col span="20" offset="2">
      <EChartLineRate id="chart_4" :rawData="rateData" title="Housing affortablity" subTitle="median annual rent rate = median annual rent / median gross annual household income * 100)%"></EChartLineRate>
      </Col>
    </Row>

  </div>
</template>
<script>
/* eslint-disable */
import EChartBar from './EChartBar'
import EChartPie from './EChartPie'
import EChartLine from './EChartLine'
import EChartLineRate from './EChartLineRate'

const rawData = [
  [
    'Year',
    'median gross annual household income',
    'median annual rent',
    'median annual mortgage payments'
  ],
  ['2001', 40768, 7540, 10404],
  ['2006', 53508, 9932, 15600],
  ['2011', 63960, 14820, 21600]
]
export default {
  components: {
    EChartBar,
    EChartPie,
    EChartLine,
    EChartLineRate
  },
  data() {
    return {
      rawData,
      showOrigin: true
    }
  },
  computed: {
    tableColumns() {
      let keyCount = 0
      return rawData[0].map(title => {
        keyCount++
        return {
          key: `key_${keyCount}`,
          title
        }
      })
    },
    tableData() {
      return rawData.slice(1).map(datas => {
        let keyCount = 0
        const dataObject = {}
        datas.forEach(data => {
          keyCount++
          dataObject[`key_${keyCount}`] = data
        })

        return dataObject
      })
    },
    rateData() {
      const rateData = [...rawData].map(data => [data[0], data[2], data[3]])
      rateData[0][1] = 'median annual rent rate'
      rateData[0][2] = 'median annual mortgage payments rate'
      for (let i = 1; i < rateData.length; i++) {
        rateData[i][1] = rawData[i][2] / rawData[i][1]
        rateData[i][2] = rawData[i][3] / rawData[i][1]
      }

      console.log(rateData)
      return rateData
    },
    pieData() {
      const pieData = [...rawData].map(data => [
        data[0],
        data[2],
        data[3],
        data[1],
        data[1]
      ])
      pieData[0][3] = 'income left'
      pieData[0][4] = 'Income left'
      for (let i = 1; i < pieData.length; i++) {
        pieData[i][3] = rawData[i][1] - rawData[i][2]
        pieData[i][4] = rawData[i][1] - rawData[i][3]
      }

      console.log('-----')
      console.log(pieData)
      return pieData
    }
  },
  methods: {
    switchView() {
      this.showOrigin = !this.showOrigin
    }
  }
}
</script>
<style>
.relative {
  position: relative;
}

.view {
  position: absolute;
  top: 30px;
  right: 30px;
  z-index: 999;
}
</style>
