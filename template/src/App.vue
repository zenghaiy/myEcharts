<template>
  <div id="app">
    <div id="header">
      <div class="title">Echarts数据可视化</div>
      <div class="now-time">当前时间：{{time}}</div>
    </div>
    <div id="box">
      <div id="left-box">
        <div class="left-top border" ref="leftTop"></div>
        <div class="left-mid border" ref="leftMid"></div>
        <div class="left-bottom border" ref="leftBottom"></div>
      </div>
      <div id="mid-box">
        <div id="mid-top">
          <div class="mid-header">
            <div class="item">125811</div>
            <div class="line">|</div>
            <div class="item">104563</div>
          </div>
          <div class="mid-header-word">
            <div class="word1">前端需求人数</div>
            <div class="word2">市场供应人数</div>
          </div>
        </div>
        <div id="mid-mid" ref="midMap"></div>
      </div>
      <div id="right-box">
        <div class="right-top border" ref="rightTop"></div>
        <div class="right-mid border" ref="rightMid"></div>
        <div class="right-bottom border" ref="rightBottom"></div>
      </div>
    </div>
  </div>
</template>

<script>
import './assets/chinaJS/china.js'
import mapData from './assets/data/mapData'
let GZData = mapData.GZData;
let BJData = mapData.BJData;
let SHData = mapData.SHData;
let planePath = mapData.planePath;
let geoCoordMap = mapData.geoCoordMap
export default {
  name: 'App',
  data() {
      return {
        time: "",
        timer: null,
      }
  },
  created() {
    let timer = setInterval(() => {
      this.getNowTime()
    }, 1000)
  },
  beforeDestroy() {
    clearInterval(this.timer)
  },
  mounted() {
    this.initLeftTop()
    this.initLeftMid()
    this.initLeftBottom()
    this.initMap()
    this.initRightTop()
    this.initRightMid()
    this.initRightBottom()
  },
  methods: {
    //对小于10的时间进行加0
    returnTenTime(value) {
      return value < 10 ? "0" + value : value
    },
    //获取当前时间
    getNowTime() {
      let time = new Date()
      let year = time.getFullYear();
      let month = this.returnTenTime(time.getMonth()+1);
      let day = this.returnTenTime(time.getDate())
      let hour = this.returnTenTime(time.getHours())
      let min = this.returnTenTime(time.getMinutes())
      let secend = this.returnTenTime(time.getSeconds())
      this.time = year + "年" + month + "月" + day + "日" + hour + "时" + min + "分" + secend + "秒"
    },
    //左上角柱状图初始化
    initLeftTop() {
      let myEcharts = this.$echarts.init(this.$refs.leftTop)
      let option = {
        title: {
          text: "柱状图-就业行业",
          textStyle: {
            color: "white",
            fontWeight: "normal"
          },
          left: "35%"
        },
        grid: {
          width: "85%",
          height: "70%"
        },
        
        xAxis: {
          type: "category",
          data: ["旅游行业", "教育培训", "游戏行业", "医疗行业", "电商行业", "社交行业", "金融行业"],
          axisLabel: {
            interval: 0, //设置X轴坐标的不隐藏，显示所有的标签
            fontSize: 11,
          },
          axisLine: {
            lineStyle: {
              color: "#b6b6b6"
            }
          }
        },
        yAxis: {
          type: 'value',
          splitLine: { //设置垂直于Y轴的刻度线的样式
            show: true,
            lineStyle: {
              color: "gray"
            }
          },
          axisLine: { //设置Y轴左边的刻度轴和轴上的文字样式
            lineStyle: {
              color: "#b6b6b6"
            }
          }
        },
        series: [
          {
            type: "bar",
            barWidth: 20,
            data: [200, 300, 300, 900, 1500, 1200, 600],
            itemStyle: {
              borderRadius: 4,
            }
          }
        ],
        tooltip: {
          show: true,
          trigger: "item", //设置鼠标hover到折线上去就进行展示X轴数据
          textStyle: {
            color: "white"
          },
          backgroundColor: "#0cc5fc"
        },
      }
      myEcharts.setOption(option)
    },
    //左边中间的折线图初始化
    initLeftMid() {
      let myEcharts = this.$echarts.init(this.$refs.leftMid);
      let option = {
        title: {
          text: "折线图-人员变化",
          textStyle: {
            color: "white",
            fontWeight: "normal"
          },
          left: "35%",
          top: "0"
        },
        legend: {
          data: ['新增粉丝', '新增游客'],
          left: "60%",
          top: "10%",
          textStyle: {
            color: "#0cc5fc"
          }
        },
        
        grid: {
          width: "85%",
          height: "70%",
          bottom: "18",
        },
        
        xAxis: {
          type: "category",
          data: ["1月", "2月", "3月", "4月", "5月", "6月", "7月", "8月", "9月", "10月", "11月", "12月"],
          axisLabel: {
            interval: 0, //设置X轴坐标的不隐藏，显示所有的标签
            fontSize: 11,
          },
        },
        yAxis: {
          type: "value",
          splitLine: { //设置垂直于Y轴的刻度线的样式
            show: true,
            lineStyle: {
              color: "gray"
            }
          },
        },
        series: [
          {
            type: "line",
            name: "新增游客",
            lineStyle: {
              color: "yellow"
            },
            data: [80, 100, 300, 450, 390, 550, 520, 505, 450, 300, 420, 400],
            smooth: true
          },
          {
            type: "line",
            name: "新增粉丝",
            data: [40, 60, 100, 120, 100, 220, 200, 220, 120, 220, 210, 200],
            smooth: true
          }
        ],
        tooltip: {
          show: true,
          trigger: "item", //设置鼠标hover到折线上去就进行展示X轴数据
          textStyle: {
            color: "white"
          },
          backgroundColor: "#0cc5fc"
        },
      }
      myEcharts.setOption(option)
    },
    //左边下面的饼图初始化
    initLeftBottom() {
      let myEcharts = this.$echarts.init(this.$refs.leftBottom)
      let option = {
        title: {
          text: "饼形图-年龄分布", //标题
          textStyle: {
            color: "white",  //标题文字的颜色
            fontWeight: "normal"
          },
          left: "35%" //相对于盒子左边的距离
        },
        legend: { //显示下方一排的区间图例
          bottom: '5%', //相对于盒子的位置
          left: 'center',
          textStyle: {
            color: "white" //文字的颜色
          },
        },
        series: [
          {
            type: "pie", //告诉Echarts要绘制一个饼图
            radius: ['40%', '70%'], //中间空心的小圆的半径，外面大圆的半径
            label: {
              show: false, //控制时候显示每一块的文字
            },
            labelLine: { //控制每一块的线条指示是否显示
              show: false
            },
            data: [
              {value: 0, name: "0岁以下", itemStyle: {color: "#5aa8f8"}}, //itemStyle设置每块的颜色
              {value: 50, name: "20-29岁", itemStyle: {color: "#1884f2"}},
              {value: 80, name: "30-39岁", itemStyle: {color: "#0572e2"}},
              {value: 110, name: "40-49岁", itemStyle: {color: "#1165bc"}},
              {value: 20, name: "50岁以上", itemStyle: {color: "#0a4e94"}},
            ]
          }
        ],
        //鼠标hover到每一块显示提示框
        tooltip: {
          show: true,
          trigger: "item", //设置鼠标hover到折线上去就进行展示X轴数据
          textStyle: {
            color: "white" //提示框文字的颜色
          },
          backgroundColor: "#0cc5fc" //提示框的背景色
        },
      }
      myEcharts.setOption(option)
    },
    //用于返回起点名字、终点名字、起点坐标、终点坐标
    returnCoords(data) {
      let result = [];
      for(let i = 0; i < data.length; i++) {
        let fromcoords = geoCoordMap[data[i][0]["name"]];
        let tocoords = geoCoordMap[data[i][1]["name"]];
        result.push({
          fromname: data[i][0]["name"],
          toname: data[i][1]["name"],
          coords: [fromcoords, tocoords],
          value: data[i][1]["value"]
        })
      }
      return result
    },
    //返回航线和飞机的颜色：三种颜色（起点为广州的、上海的、北京的）
    returnColor(index) {
      return index === 0 ? "#6beef4" : (index === 1 ? "#6beef4" : "#6beef4")
    },
    //初始化地图
    initMap() {
      let series = [];
      [GZData, SHData, BJData].forEach((item, index) => {
        series.push({
            type: "lines", //series.type = "lines"表名是想渲染模拟迁徙的模板
            name: index === 0 ? "广州" : (index === 1 ? "上海" : "北京"), //鼠标hover到航线上去显示不同的内容
            effect: {
              show: true,
              symbol: planePath, //把矢量图planPath赋值给symbol
              symbolSize: 18 // 控制矢量图的大小
            },
            //线条的样式
            lineStyle: {
              curveness: 0.2, //控制线条的弯曲度
              color: this.returnColor(index),//不同的起点显示不同的线条样式
              width: 3 //线条的宽度
            },
            data: this.returnCoords(item) //所有的航线起始地点的数据
        })
      })
      let myEcharts = this.$echarts.init(this.$refs.midMap)
      let option = {
        //geo用于显示地图坐标
        geo: {
          map: "china", //导入的china.js，该文件中注册的china地图。
          zoom: 1.2, //默认显示的缩放倍数
          roam: true, //是否开启鼠标的缩放
          scaleLimit: { //控制鼠标缩放的最小倍数，最大倍数
            min: 1,
            max: 1
          },

          label: {
            show: true, //控制省份名称的显示隐藏
            offset: [0, 0], // 控制省份名称的偏移量
            color: "white" // 省份名称的字体颜色
          },

          //板块的颜色
          itemStyle: {
                areaColor: 'rgba(38, 125, 225,0.8)', //每个板块的颜色
                borderColor: '#404a59' //板块的边界颜色
          },
          //鼠标移入某板块的样式设置
          emphasis: {
            //移入该板块时的板块颜色
            itemStyle: {
              areaColor: '#2BBFF7',
            },
            //省份的名称颜色
            label: {
              color: "white"
            }
          },
      },
      series: series, //显示的数据

      //鼠标hover到航线上显示数据
      tooltip: {
        show: true,
        formatter: function (data){ //返回该航线的数据
          return data.data.fromname + ">>>" + data.data.toname+"："+data.data.value
        }
      },
      }
      myEcharts.setOption(option)
    },
    initRightTop() {
      let myEcharts = this.$echarts.init(this.$refs.rightTop);
      let option = {
        title: {
          text: "柱状图-能力掌握",
          textStyle: {
            color: "white",
            fontWeight: "normal"
          },
          left: "35%"
        },
        xAxis: {
          show: false
        },
        yAxis: [
          {
            type: "category",
            inverse: true,
            data: ["HTML5", "CSS3", "javascript", "VUE", "NODE"],
            // 不显示y轴的线
            axisLine: {
              show: false
            },
            // 不显示刻度
            axisTick: {
              show: false
            },
            // 把刻度标签里面的文字颜色设置为白色
            axisLabel: {
              color: "#fff"
            }
          },
          {
            data: [702, 350, 610, 793, 664],
            inverse: true,
            axisLine: { //设置不显示Y轴的线
              show: false
            },
            axisTick: {//设置不显示刻度
              show: false
            },
            // 把刻度标签里面的文字颜色设置为白色
            axisLabel: {
              color: "#fff"
            }
          }
        ],
        //grid可控制整个组件的大小
        grid: {
          width: "75%",
          left: "17%"
        },
        series: [
          //设置每一条的内部填充
          {
            type: "bar",
            data: [
              {
              value: 70,
              itemStyle: {
                color: "yellow",
                borderRadius: 10
              }
              },
              {
              value: 60,
              itemStyle: {
                color: "#0cc5fc",
                borderRadius: 10
              }
              },
              {
              value: 50,
              itemStyle: {
                color: "purple",
                borderRadius: 10
              }
              },
              {
              value: 80,
              itemStyle: {
                color: "pink",
                borderRadius: 10
              }
              },
              {
              value: 50,
              itemStyle: {
                color: "#20e27799",
                borderRadius: 10
              }
              }
            ],
            barWidth: 20, //设置柱子的宽度
          },
          {
            //设置每一条的外框
            type: "bar",
            barWidth: 25,
            yAxisIndex: 1,
            data: [100, 100, 100, 100, 100], //然后数据拉满
            itemStyle: {
              color: "none", //把内部设置为无色，然后设置边框的颜色，可实现边框
              borderColor: "#00c1de",
              borderWidth: 3,
              borderRadius: 15
            },
            
          }
        ]
      }
      myEcharts.setOption(option)
    },
    //右边中间部分的折线图实现
    initRightMid() {
      let myEcharts = this.$echarts.init(this.$refs.rightMid)
      let option = {
        color: ["red", "blue"],
        title: {
          text: "折线图-播放量",
          textStyle: {
            color: "white",
            fontWeight: "normal"
          },
          left: "35%"
        },
        legend: {
          data: ['播放量', '转发量'],
          left: "60%",
          top: "10%",
          textStyle: {
            color: "#0cc5fc"
          }
        },
        
        grid: {
          width: "85%",
          height: "70%",
          bottom: "18",
        },
        
        xAxis: {
          type: "category",
          data: ["1月", "2月", "3月", "4月", "5月", "6月", "7月", "8月", "9月", "10月", "11月", "12月"],
          axisLabel: {
            interval: 0, //设置X轴坐标的不隐藏，显示所有的标签
            fontSize: 11,
          },
        },
        yAxis: {
          type: "value"
        },
        series: [
          {
            type: "line",
            name: "播放量",
            lineStyle: {
              color: "yellow"
            },
            data: [80, 100, 300, 450, 390, 550, 520, 505, 450, 300, 420, 400],
            smooth: true,
            stack: "Total",
            areaStyle: {
              opacity: 0.8,
              color: new this.$echarts.graphic.LinearGradient(0, 0, 0, 1, [
                {
                  offset: 0,
                  color: 'rgba(128, 255, 165)'
                },
                {
                  offset: 1,
                  color: 'rgba(1, 191, 236)'
                }
              ])
            },
            emphasis: {
              focus: 'series'
            },
          },
          {
            type: "line",
            name: "转发量",
            data: [40, 60, 100, 120, 100, 220, 200, 220, 120, 220, 210, 200],
            smooth: true,
            stack: "Total",
            areaStyle: {
              opacity: 0.8,
              color: new this.$echarts.graphic.LinearGradient(0, 0, 0, 1, [
                {
                  offset: 0,
                  color: 'rgba(55, 162, 255)'
                },
                {
                  offset: 1,
                  color: 'rgba(116, 21, 219)'
                }
              ])
            },
            emphasis: {
              focus: 'series'
            },
          }
        ]
      }
      myEcharts.setOption(option)
    },
    initRightBottom() {
      let myEcharts = this.$echarts.init(this.$refs.rightBottom);
      let option = {
        title: {
          text: "饼形图-地区分布",
          textStyle: {
            color: "white",
            fontWeight: "normal"
          },
          left: "35%"
        },
        legend: { //控制下方的图例的显示
          bottom: '0%',
          left: 'center',
          textStyle: {
            color: "white"
          },
        },
        series: [
          {
            name: '55',
            type: 'pie',
            radius: [20, "60%"],
            roseType: 'area',
            width: "100%",
            height: "94%",
            center: ['50%', '50%'],
            roseType: 'area',
            // itemStyle: {
            //   borderRadius: 8
            // },
            label: {
              color: "#0cc5fc"
            },
            data: [
              { value: 40, name: '广东' },
              { value: 38, name: '四川' },
              { value: 32, name: '上海' },
              { value: 30, name: '浙江' },
              { value: 28, name: '江苏' },
              { value: 26, name: '重庆' },
              { value: 22, name: '北京' },
              { value: 18, name: '湖北' }
            ]
          }
        ],
        tooltip: {
          show: true,
          trigger: "item",
          textStyle: {
            color: "white" //提示框的文字的颜色
          },
          backgroundColor: "#0cc5fc" //提示框的背景
        }
      };
      myEcharts.setOption(option);
    }
  },
}
</script>

<style>
@font-face {
  font-family: "led regular";
  src: url("./assets/FontFamily/Open\ 24\ Display\ St.ttf");
}
html,body,#app {
  width: 100%;
  height: 100%;
  margin: 0;
  padding: 0;
  background-image: url("./assets/bg.jpg");
  background-repeat: no-repeat;
  background-size: cover;
  position: relative;
  z-index: -222;
}
.title {
  color: white;
  font-size: 30px;
  text-align: center;
}
.now-time {
  color: aqua;
  float: right;
  font-size: 20px;
  margin-right: 10px;
  margin-top: -20px;
}
#box {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: row;
  position: relative;
  z-index: -10;
}
#header {
  width: 100%;
  height: 105px;
  background-image: url("./assets/head_bg.png");
  background-repeat: no-repeat;
  background-size: cover;
  position: relative;
  z-index: -1;
}
#left-box {
  width: 25%;
  height: 100%;

}
#mid-box {
  width: 50%;
  height: 100%;
}
#right-box {
  width: 25%;
  height: 100%;
}
.left-top {
  margin-top: 5px;
  margin-left: 5px;
  width: 99%;
  height: 31%;
}
.left-mid {
  margin-top: 15px;
  margin-left: 5px;
  width: 99%;
  height: 31%;
}
.left-bottom {
  margin-top: 15px;
  margin-left: 5px;
  width: 99%;
  height: 31%;
}
#mid-top {
  width: 94%;
  height: 16%;
  margin-left: 3%;
  margin-top: 1%;
  background-color: #0f226d99;
}
.mid-header {
  width: 90%;
  height: 65%;
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  margin-left: 5%;
  border: 1px solid rgba(73, 73, 73,0.5);
  background: linear-gradient(to left, #0cc5fc, #0cc5fc) left top no-repeat,        /*设置左上角下边的边角 */
                linear-gradient(to bottom, #0cc5fc, #0cc5fc) left top no-repeat,    /*设置左上角的边角 */
                linear-gradient(to left, #0cc5fc, #0cc5fc) right bottom no-repeat,        /*设置右下角下边的边角 */
                linear-gradient(to left, #0cc5fc, #0cc5fc) right bottom no-repeat;
  background-size: 2px 10px, 20px 2px, 2px 10px, 20px 2px;
}
.item {
  color: yellow;
  font-family: "led regular";
  font-size: 60px;

}
.line {
  color: rgba(73, 73, 73,0.5);
  font-size: 40px;
  margin-top: 10px;
}
.mid-header-word {
  font-size: 20px;
  color: gray;
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  margin-top: 10px;
}
.word1 {
  margin-right: 100px;
}
.word2 {
  margin-left: -30px;
}
#mid-mid {
  width: 96%;
  margin-left: 2%;
  margin-top: 2%;
  height: calc(100% - 20%);
  background-image: url('./assets/map.png');
  background-size: 78%;
  background-repeat: no-repeat;
  background-position-x: center;
  background-position-y: top;
}
.right-top {
  margin-top: 5px;
  margin-right: 5px;
  width: 99%;
  height: 31%;
}
.right-mid,.right-bottom{
  margin-top: 15px;
  margin-right: 5px;
  width: 99%;
  height: 31%;
}
</style>
<style>
.border {
  background:
                linear-gradient(to left, #0cc5fc, #0cc5fc) left top no-repeat,        /*设置左上角下边的边角 */
                linear-gradient(to bottom, #0cc5fc, #0cc5fc) left top no-repeat,    /*设置左上角的边角 */
                linear-gradient(to left, #0cc5fc, #0cc5fc) right top no-repeat,     /*设置右上角下边的边角 */
                linear-gradient(to bottom, #0cc5fc, #0cc5fc) right top no-repeat,   /*设置右上角上边的边角 */
                linear-gradient(to left, #0cc5fc, #0cc5fc) left bottom no-repeat,   /*设置左下角上边的边角 */
                linear-gradient(to bottom, #0cc5fc, #0cc5fc) left bottom no-repeat, /*设置左下角下边的边角 */
                linear-gradient(to left, #0cc5fc, #0cc5fc) right bottom no-repeat,        /*设置右下角下边的边角 */
                linear-gradient(to left, #0cc5fc, #0cc5fc) right bottom no-repeat;        /*设置右下角上边的边角 */
    background-size: 2px 20px, 20px 2px, 2px 20px, 20px 2px;  /*所有的左边 宽长， 所有的上边 宽长，所有的右边 宽长，所有的下边 宽长，*/ 
}
</style>
