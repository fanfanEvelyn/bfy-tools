<template>
  <div>
    <div class="container">
      <div id="echart" ref="chart" class="echart1" />
      <div id="echart2" class="echart2" />
      <div class="clear" />
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      myChart: {}
    }
  },
  computed: {
    origin() {
      return this.data
    },
    opt() {
      const obj = {
        title: {
          text: '网站数据来源',
          x: 'center'
        },
        tooltip: {
          trigger: 'item',
          formatter: '{a} <br/>{b} : {c} ({d}%)'
        },
        legend: {
          orient: 'vertical',
          left: 'left',
          data: ['直接访问', '邮件营销', '联盟广告', '视频广告', '搜索引擎']
        },
        series: [
          {
            name: '访问来源',
            type: 'pie',
            radius: '55%',
            center: ['50%', '60%'],
            data: [
              { value: 335, name: '直接访问' },
              { value: 310, name: '邮件营销' },
              { value: 234, name: '联盟广告' },
              { value: 135, name: '视频广告' },
              { value: 1548, name: '搜索引擎' }
            ],
            itemStyle: {
              emphasis: {
                shadowBlur: 10,
                shadowOffsetX: 0,
                shadowColor: 'rgba(0, 0, 0, 0.5)'
              }
            }
          }
        ]
      }
      return obj
    },
    option() {
      const option = {
        title: {
          text: '浏览器占比变化',
          subtext: '纯属虚构',
          top: 10,
          left: 10
        },
        tooltip: {
          trigger: 'item',
          backgroundColor: 'rgba(0,0,250,0.2)'
        },
        legend: {
          type: 'scroll',
          bottom: 10,
          data: (function() {
            var list = []
            for (var i = 1; i <= 28; i++) {
              list.push(i + 2000 + '')
            }
            return list
          })()
        },
        visualMap: {
          top: 'middle',
          right: 10,
          color: ['red', 'yellow'],
          calculable: true
        },
        radar: {
          indicator: [
            { text: 'IE8-', max: 400 },
            { text: 'IE9+', max: 400 },
            { text: 'Safari', max: 400 },
            { text: 'Firefox', max: 400 },
            { text: 'Chrome', max: 400 }
          ]
        },
        series: (function() {
          var series = []
          for (var i = 1; i <= 28; i++) {
            series.push({
              name: '浏览器（数据纯属虚构）',
              type: 'radar',
              symbol: 'none',
              lineStyle: {
                width: 1
              },
              emphasis: {
                areaStyle: {
                  color: 'rgba(0,250,0,0.3)'
                }
              },
              data: [
                {
                  value: [
                    (40 - i) * 10,
                    (38 - i) * 4 + 60,
                    i * 5 + 10,
                    i * 9,
                    i * i / 2
                  ],
                  name: i + 2000 + ''
                }
              ]
            })
          }
          return series
        })()
      }
      return option
    }
  },
  mounted() {
    this.setChats()
  },
  methods: {
    setChats() {
      const myChart = this.$echarts.init(document.getElementById('echart'))
      const myChart2 = this.$echarts.init(document.getElementById('echart2'))
      myChart.setOption(this.opt)
      myChart2.setOption(this.option)
    }
  }
}
</script>
<style>
.container {
  width: 100%;
}
.clear {
  clear: both;
}
.echart1,
.echart2 {
  width: 500px;
  height: 400px;
  margin: 50px;
  float: left;
}
</style>
