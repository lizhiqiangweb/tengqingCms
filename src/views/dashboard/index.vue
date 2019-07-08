<template>
<div class="dashboard-container">
  <!-- msg -->
  <el-row :gutter="20">
    <el-col class="el-msg" v-for="(item, index) in iconList" :key="index" :span="6">
      <img :src="item.url" alt="">
      <div class="msg left">
        <h4>{{item.name}}</h4>
        <span>{{item.data}}</span>
      </div>
    </el-col>
  </el-row>
  <!-- echarts -->
  <el-row>
    <el-col id="myChart" :style="{width: '100%', height: '36em'}" :span="24"></el-col>
  </el-row>
  <!-- panel -->
  <el-row class="panel" :gutter="20">
    <el-col v-for="(item, index) in newsList" :key="index" :span="8">
      <div class="panel-container">
        <h4><i :class="item.iconClass"></i>{{item.tit}}</h4>
        <ul>
          <li v-for="(item1, index) in item.data" :key="index">
            <p>{{item1}}</p>
            <div><i @click="cancel(scope.$index, scope.row)" class="fa fa-times"></i></div>
          </li>
        </ul>
      </div>
    </el-col>
  </el-row>
  
</div>
</template>

<script>
export default {
  name: 'Dashboard',
  data() {
    return {
      iconList: [{
          name: '用户访问量',
          url: require('@/assets/img/icon_visit.png'),
          data: 1000
        },
        {
          name: '信息',
          url: require('@/assets/img/icon_info.png'),
          data: 0
        }
      ],
      newsList: [
        {
          tit: '最新消息',
          iconClass: 'fa fa-newspaper-o',
          data: [
            '最新消息1',
            '最新消息2'
          ]
        },
        {
          tit: '我的发布',
          iconClass: 'fa fa-map-o',
          data: [
            '新闻1',
            '最新公告'
          ]
        },
        {
          tit: '操作处理',
          iconClass: 'fa fa-cog',
          data: [
            '更新了文章1',
            '删除了文章2'
          ]
        }
      ],
      myChart: null,
    }
  },
  mounted() {
    this.drawLine()
  },
  methods: {
    drawLine() {
      // 基于准备好的dom，初始化echarts实例
      let myChart = this.$echarts.init(document.getElementById('myChart'))
      // 绘制图表
      myChart.setOption({
        title: {
          text: ''
        },
        tooltip: {
          trigger: 'axis'
        },
        legend: {
          data: ['用户访问量', '信息']
        },
        grid: {
          left: '3%',
          right: '4%',
          bottom: '3%',
          containLabel: true
        },
        toolbox: {
          feature: {
            saveAsImage: {}
          }
        },
        xAxis: {
          type: 'category',
          boundaryGap: false,
          data: ['2019.7.1', '2019.7.2', '2019.7.3', '2019.7.4', '2019.7.5', '2019.7.6', '2019.7.7']
        },
        yAxis: {
          type: 'value'
        },
        series: [{
            name: '用户访问量',
            type: 'line',
            stack: '总量',
            data: [120, 132, 141, 144, 190, 230, 240]
          },
          {
            name: '信息',
            type: 'line',
            stack: '总量',
            data: [6, 10, 5, 20, 26, 14, 18]
          }
        ]
      })
      window.addEventListener('resize', function () {
        myChart.resize();
      })
    },
    cancel:function(index, r) {
      app.tableData.splice(index,1)
    }
  }
}
</script>

<style lang="scss" scoped>
.dashboard {
  &-container {
    margin: 30px;
  }
}

.el-msg {
  background: #fff;
  padding: 15px 24px !important;
  margin: 0 24px 0 0;

  img {
    width: 64px;
    height: 64px;
    float: left;
  }

  .msg {
    margin: 16px 0 0 0;
    float: right;
    text-align: center;

    span {
      color: #333;
      font-size: 14px;
    }
  }
}

#myChart {
  margin: 12px 0 0 0;
  background: #fff;
  padding: 24px 0;
}

.panel {
  margin: 24px 0 0 0;
  &-container {
    background: #fff;
    border-radius: 10px;
    h4 {
      padding: 10px 24px;
      border-bottom: 1px solid #aaa;
      i {
        margin: 0 8px 0 0;
      }
    }
    ul {
      padding: 12px 30px 24px 32px;
      li {
        line-height: 1.6;
        font-size: 14px;
        p {
          float: left;
        }
        div {
          float: right;
        }
      }
      li::after {
        content: '';
        width: 0;
        height: 0;
        display: block;
        clear: both;
      }
    }
  }
}
</style>
