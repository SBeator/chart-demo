<template>
  <div>
    <Row>
      <br/>
    </Row>
    <Row>
      <Col span="20" offset="2">
      <Table :columns="tableColumns" :data="tableData"></Table>
      </Col>
    </Row>
    <Row>
      <Col span="20" offset="2">
      <EChartBar id="chart_1" :rawData="rawData"></EChartBar>
      </Col>
    </Row>
    <Row>
      <Col span="20" offset="2">
      <EChartLine id="chart_2" :rawData="rawData"></EChartLine>
      </Col>
    </Row>
    <Row>
      <Col span="20" offset="2">
      <EChartPie id="chart_3" :rawData="rawData"></EChartPie>
      </Col>
    </Row>
  </div>
</template>
<script>
/* eslint-disable */
import EChartBar from './EChartBar'
import EChartPie from './EChartPie'
import EChartLine from './EChartLine'

const rawData = [
  [
    'Year',
    'median	gross	annual	household	income',
    'median	annual	rent',
    'median	annual	mortgage	payments'
  ],
  ['2001', 40768, 7540, 10404],
  ['2006', 53508, 9932, 15600],
  ['2011', 63960, 14820, 21600]
]
export default {
  components: {
    EChartBar,
    EChartPie,
    EChartLine
  },
  data() {
    return {
      rawData
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
    }
  },
  methods: {}
}
</script>
