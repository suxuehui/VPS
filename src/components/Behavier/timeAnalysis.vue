<!-- 数据图表-历史轨迹 -->
<template>
  <section id="timeAnalysis">
    <div class="left-top" style="z-index:2"></div>
    <div class="right-top" style="z-index:2"></div>
    <div class="let-bottom" style="z-index:2"></div>
    <div class="right-bottom" style="z-index:2"></div>
    <div class="header">
      <span>历史轨迹</span>
    </div>
    <div id='timeChart'></div>
    <!-- <div :is="mapComponent" class="hismap" :mapid="'maphis'+vehicleid" ref="hmap"></div> -->
  </section>
</template>

<script>
  // import comFrameBox from '@/components/MessageBox/comFrameBox' //弹框公用头部
  import '../../../static/script/china.js'// 引入地图
  import { historyDraw } from '@/Api/mapApi.js'
  export default {
    name: '',
    // props:['currentIndex','vehicleid'],
    // components: {
    //   amap: resolve => {
    //     require(['@/components/Map/amap'], resolve)
    //   },
    //   "com-frame-box":comFrameBox
    // },
    computed: {
      // //初始化地图类型
      // maptype() {
      //   return this.$store.getters.MAPTYPE;
      // },
      // //地图类型
      // mapComponent() {
      //   return this.maptype == 0 ? 'amap' : 'bmap';
      // },
      timeState () {
        return this.$store.getters.timeState;
      }
    },
    data () {
      return {
        listData: [] // 数据组
      }
    },
    mounted () {
      let _this = this;
      _this.getDataDetail((data) => { // 获取数据
        _this.drawLine(1, data);
      })
    },
    methods: {
      // 获取数据
      getDataDetail (callback) {
        var reg = /[a-zA-Z]/g; var  carid = ''; // 去掉车辆id中的字母
        carid = this.$store.state.hisZoom.info[0].vehicleid.replace(reg, '');
        let para = {
          carId: carid,
          type: this.$store.getters.timeState
        }
        historyDraw(para).then((res) => {
          this.listData = res.data.data;
          if (callback) callback(this.listData);
        })
      },


      drawLine (timeState, listData) {
        let chartData = ''; let _this = this;
        switch (+timeState) {
          // 年
          case 1: {
            chartData = (function () {
              var res = [];
              if (listData.length != 0) {
                listData.forEach((item, index) => {
                  res.push({value: [item.LNG, item.LAT]});
                });
              }
              return res;
            })();
            _this.chartInit(chartData);
            break;
          }
          // 月
          case 2: {
            chartData = (function () {
              var res = [];
              if (listData.length != 0) {
                listData.forEach((item, index) => {
                  res.push({value: [item.LNG, item.LAT]});
                });
              }
              return res;
            })();
            _this.chartInit(chartData);
            break;
          }
          // 日
          case 3: {
            chartData = (function () {
              var res = [];
              if (listData.length != 0) {
                listData.forEach((item, index) => {
                  res.push({value: [item.LNG, item.LAT]});
                });
              }
              return res;
            })();
            _this.chartInit(chartData);
            break;
          }
        }
      },
      chartInit (chartData) {
        let _this = this;
        // 基于准备好的dom，初始化echarts实例
        let parentDiv = document.querySelector('#timeChart');
        // 自适应宽高
        let myChartContainer = function () {
          parentDiv.style.width = $('#timeAnalysis').width() + 'px';
          parentDiv.style.height = $('#timeAnalysis').height() + 'px';
        };
        myChartContainer();
        let timeChart = _this.$echarts.init(document.getElementById('timeChart'));
        timeChart.setOption({
          tooltip: {
            trigger: 'item',
            formatter: function (params) {
              return '经度: ' + params.value[0] + ' / 纬度: ' + params.value[1];
            }
          },
          geo: {
            map: 'china',
            roam: true, // 是否缩放
            scaleLimit: {// 滚轮最大最小缩放值
              min: 1
            },
            label: {// 省名称
              emphasis: {
                show: true,
                color: '#0C243F'
              }
            },
            itemStyle: {
              emphasis: {
                areaColor: '#99A8B9',
                opacity: 0.9
              },
              areaColor: '#D4D4D4',
              opacity: 0.8
            }
          },
          series: [
            {
              name: '历史轨迹',
              type: 'scatter',
              symbolSize: 5, // 散点大小
              coordinateSystem: 'geo',
              data: chartData
            }
          ]
        });

        // 浏览器大小改变时重置大小
        window.addEventListener('resize', function () {
          myChartContainer();
          timeChart.resize();
        });
      }
    },
    watch: {
      timeState: function (newVal, oldVal) {
        this.getDataDetail((data) => { // 获取数据
          this.drawLine(newVal, data);
        });
      }
    }
  }
</script>
