<template>
  <div ref="dom" class="charts chart-line"></div>
</template>

<script>
import echarts from 'echarts'
import tdTheme from './theme.json'
import { on, off } from '@/libs/tools'
echarts.registerTheme('tdTheme', tdTheme)
export default {
  props: {
    title: Array,
    data1: Array,
    data2: Array,
    data3: Array,
    text: String
  },
  data () {
    return {
      dom: null
    }
  },
  methods: {
    resize () {
      this.dom.resize()
    },
    init () {
      let xAxisData = this.data1
      let seriesData1 = this.data2
      let seriesData2 = this.data3
      let option = {
        title: {
          text: this.title[0],
          subtext: '',
          left: 'center',
          bottom: '5%'
        },
        tooltip: {
          trigger: 'axis',
          axisPointer: {
            type: 'cross'
          }
        },
        legend: {
          data: this.title
        },
        toolbox: {
          show: true
        },
        xAxis: {
          type: 'category',
          boundaryGap: false,
          zlevel: 1,
          data: xAxisData
        },
        yAxis: {
          type: 'value',
          axisLabel: {
            formatter: '{value}'
          },
          axisPointer: {
            snap: true
          }
        },
        series: [
          {
            name: this.title[1],
            data: seriesData1,
            type: 'line',
            smooth: true,
            itemStyle: {
              normal: {
                color: '#97d4b7'
              }
            },
            lineStyle: {
              normal: {
                color: '#97d4b7'
              }
            },
            areaStyle: {
              normal: {
                color: '#97d4b7',
                opacity: 0.8
              }
            }
          },
          {
            name: this.title[2],
            data: seriesData2,
            type: 'line',
            smooth: true,
            itemStyle: {
              normal: {
                color: '#d4d098'
              }
            },
            lineStyle: {
              normal: {
                color: '#d4d098'
              }
            },
            areaStyle: {
              normal: {
                color: '#d4d098',
                opacity: 0.8
              }
            }
          }
        ]
      }
      this.dom = echarts.init(this.$refs.dom, 'tdTheme')
      this.dom.setOption(option)
    }
  },
  mounted () {
    this.$nextTick(() => {
      this.init()
      on(window, 'resize', this.resize)
    })
  },
  watch: {
    data1: {
      handler (newVal, oldVal) {
        this.init()
      },
      deep: true
    }
  },
  beforeDestroy () {
    off(window, 'resize', this.resize)
  }
}
</script>
