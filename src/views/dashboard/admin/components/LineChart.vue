<template>
  <div :class="className" :style="{height:height,width:width}"/>
</template>

<script>
import echarts from 'echarts'
require('echarts/theme/macarons') // echarts theme
import { debounce } from '@/utils'

export default {
  props: {
    className: {
      type: String,
      default: 'chart'
    },
    width: {
      type: String,
      default: '100%'
    },
    height: {
      type: String,
      default: '350px'
    },
    autoResize: {
      type: Boolean,
      default: true
    },
    chartData: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      chart: null
    }
  },
  watch: {
    chartData: {
      deep: true,
      handler(val) {
        this.setOptions(val)
      }
    }
  },
  mounted() {
    this.initChart()
    if (this.autoResize) {
      this.__resizeHandler = debounce(() => {
        if (this.chart) {
          this.chart.resize()
        }
      }, 100)
      window.addEventListener('resize', this.__resizeHandler)
    }

    // 监听侧边栏的变化
    const sidebarElm = document.getElementsByClassName('sidebar-container')[0]
    sidebarElm.addEventListener('transitionend', this.sidebarResizeHandler)
  },
  beforeDestroy() {
    if (!this.chart) {
      return
    }
    if (this.autoResize) {
      window.removeEventListener('resize', this.__resizeHandler)
    }

    const sidebarElm = document.getElementsByClassName('sidebar-container')[0]
    sidebarElm.removeEventListener('transitionend', this.sidebarResizeHandler)

    this.chart.dispose()
    this.chart = null
  },
  methods: {
    sidebarResizeHandler(e) {
      if (e.propertyName === 'width') {
        this.__resizeHandler()
      }
    },
    setOptions({ MRojali, SulistyoJati, AkhmadAnggoro, AlodiaYusuf, MAzkaPutra, perempuan, lakilaki, sah, nonsah } = {}) {
      this.chart.setOption({
        xAxis: {
          data: ['Wil.01', 'Wil.02', 'Wil.03', 'Wil.04', 'Wil.05', 'Wil.06', 'Wil.07'],
          boundaryGap: false,
          axisTick: {
            show: false
          }
        },
        grid: {
          left: 10,
          right: 10,
          bottom: 20,
          top: 30,
          containLabel: true
        },
        tooltip: {
          trigger: 'axis',
          axisPointer: {
            type: 'cross'
          },
          padding: [5, 10]
        },
        yAxis: {
          axisTick: {
            show: false
          }
        },
        series: [{
          name: 'M. Rojali', itemStyle: {
            normal: {
              color: '#2dc7c9',
              lineStyle: {
                color: '#2dc7c9',
                width: 2
              }
            }
          },
          smooth: true,
          type: 'line',
          data: MRojali,
          animationDuration: 2800,
          animationEasing: 'cubicInOut'
        },
        {
          name: 'Sulistyo Jati',
          smooth: true,
          type: 'line',
          itemStyle: {
            normal: {
              color: '#b6a2df',
              lineStyle: {
                color: '#b6a2df',
                width: 2
              }
            }
          },
          data: SulistyoJati,
          animationDuration: 2800,
          animationEasing: 'quadraticOut'
        },
        {
          name: 'Akhmad Anggoro',
          smooth: true,
          type: 'line',
          itemStyle: {
            normal: {
              color: '#59b1f0',
              lineStyle: {
                color: '#59b1f0',
                width: 2
              }
            }
          },
          data: AkhmadAnggoro,
          animationDuration: 2800,
          animationEasing: 'cubicInOut'
        },
        {
          name: 'Alodia Yusuf',
          smooth: true,
          type: 'line',
          itemStyle: {
            normal: {
              color: '#ffb780',
              lineStyle: {
                color: '#ffb780',
                width: 2
              }
            }
          },
          data: AlodiaYusuf,
          animationDuration: 2800,
          animationEasing: 'cubicInOut'
        },
        {
          name: 'M. Azka Putra',
          smooth: true,
          type: 'line',
          itemStyle: {
            normal: {
              color: '#d97a7e',
              lineStyle: {
                color: '#d97a7e',
                width: 2
              }
            }
          },
          data: MAzkaPutra,
          animationDuration: 2800,
          animationEasing: 'cubicInOut'
        },
        {
          name: 'Perempuan',
          smooth: true,
          type: 'line',
          itemStyle: {
            normal: {
              color: '#cc3333',
              lineStyle: {
                color: '#cc3333',
                width: 2
              }
            }
          },
          data: perempuan,
          animationDuration: 2800,
          animationEasing: 'cubicInOut'
        },
        {
          name: 'Laki-laki',
          smooth: true,
          type: 'line',
          itemStyle: {
            normal: {
              color: '#3333cc',
              lineStyle: {
                color: '#3333cc',
                width: 2
              }
            }
          },
          data: lakilaki,
          animationDuration: 2800,
          animationEasing: 'cubicInOut'
        },
        {
          name: 'Sah',
          smooth: true,
          type: 'line',
          itemStyle: {
            normal: {
              color: '#3333cc',
              lineStyle: {
                color: '#3333cc',
                width: 2
              }
            }
          },
          data: sah,
          animationDuration: 2800,
          animationEasing: 'cubicInOut'
        },
        {
          name: 'Tidak Sah',
          smooth: true,
          type: 'line',
          itemStyle: {
            normal: {
              color: '#cc3333',
              lineStyle: {
                color: '#cc3333',
                width: 2
              }
            }
          },
          data: nonsah,
          animationDuration: 2800,
          animationEasing: 'cubicInOut'
        }]
      })
    },
    initChart() {
      this.chart = echarts.init(this.$el, 'macarons')
      this.setOptions(this.chartData)
    }
  }
}
</script>
