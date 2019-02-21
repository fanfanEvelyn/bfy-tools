<template>
  <div id="echart" ref="chart" class="echart" />
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
    }
  },
  mounted() {
    this.setChats()
  },
  methods: {
    setChats() {
      const myChart = this.$echarts.init(document.getElementById('echart'))
      myChart.setOption(this.opt)
    }
  }
}
</script>
<style>
.echart {
  width: 500px;
  height: 400px;
  margin: 30px auto;
}
</style>
