<template>
  <div class='container'>
    <button open-type="getUserInfo" bindgetuserinfo="bindGetUserInfo" v-if="!userInfo">获取登录信息</button>
    <div class="header">
      <div class="header-info">
        <img :src="userInfo.avatarUrl" />
        <p>早上好,{{userInfo.nickName}}医生,您已经100小时没登录了，一下是您需要处理的内容。</p>
        <div class="search"><i-icon type="search" size="28" color="#80848f" /></div>
      </div>
      <div class="header-message">
        <div class="message">
          <p>已随访800人次</p>
          <p>共500位患者</p>
        </div>
        <div class="message">
          <p><span class="number">156</span></p>
          <p>随访计划<span class="name">待审核</span></p>
        </div>
        <div class="message">
          <p><span class="number">49</span></p>
          <p>随访记录<span class="name">待处理</span></p>
        </div>
      </div>
    </div>
    <!-- 疾病分类 -->
    <div class="pie-content">
      <div class="diseease-info">
        <div class="title">疾病分类情况</div>
        <div class="select">
          <picker class="weui-btn" @change="PickerChange" :value="indexPicker" :range="array">
              <view class="picker">
                筛选：<span>{{selectTime}}</span>
              </view>
            </picker>
        </div>
      </div>
      <div class="disease-echart">
        <ec-canvas class="canvas" id="mychart-dom-bar" canvas-id="mychart-bar" :ec="ec"></ec-canvas>
      </div>
    </div>
    <!-- 用药依从性 -->
    <div class="pie-content">
      <div class="diseease-info">
        <div class="title">用药依从性</div>
        <div class="select">
          <picker class="weui-btn" @change="PickerChange" :value="indexPicker" :range="array">
              <view class="picker">
                筛选：<span>{{selectTime}}</span>
              </view>
            </picker>
        </div>
      </div>
      <div class="medicine-echart">
        <ec-canvas class="canvas" id="mychart-dom-bar" canvas-id="mychart-bar" :ec="ec"></ec-canvas>
      </div>
    </div>
    <!-- 随访户数量统计 -->
    <div class="line-content">
      <div class="diseease-info">
        <div class="title">随访户数量统计</div>
        <div class="select">
          <picker class="weui-btn" @change="PickerChange" :value="indexPicker" :range="array">
            <view class="picker">
              筛选：<span>{{selectTime}}</span>
            </view>
            </picker>
        </div>
      </div>
      <div class="medicine-echart">
        <ec-canvas class="canvas" id="mychart-dom-bar" canvas-id="mychart-bar" :ec="ecLine"></ec-canvas>
      </div>
    </div>
    <!-- 特别关心 -->
    <div class="care-content">
      <div class="diseease-info">
        <div class="title">特别关注</div>
        <div class="select">
          更多
        </div>
      </div>
      <div class="care-list">
        <i-row v-for="i in 10" :key="i">
          <i-col span="4" i-class="col-class">1.张三丰</i-col>
          <i-col span="4" i-class="col-class">男/50</i-col>
          <i-col span="10" i-class="col-class">呼吸道感染感染感染</i-col>
          <i-col span="6" i-class="col-class">
            <i-tag  class="i-tags"  name="1"  color="blue">重型糖尿病</i-tag>
            <i-icon type="enter" />
          </i-col>
        </i-row>
      </div>
    </div>
    <a href='/pages/counter/index' class='counter'>去往Vuex示例页面</a>
    <i-toast id="toast" ref="toast" />
    <i-message id="message" />
  </div>
</template>

<script>
const options = {
  backgroundColor: "#fff",
  color: ["#f36837", "#5e65e3", "#5e98e3", "#59d0bd", "#f9df94", "#0f0"],
  tooltip: {
    trigger: 'item',
    formatter: "{a} <br/>{b}: {c} ({d}%)"
  },
  legend: {
    orient: 'vertical',
    left: '65%',
    y: 'middle',
    data: ['北京', '武汉', '杭州', '广州', '上海'],
    textStyle: {
      color: "#666",
      fontWeight: 'normal'
    }
  },
  grid: {
    top: '0%',
    left: '0%',
    containLabel: true,
  },
  series: [{
    label: {
      show: false
    },
    type: 'pie',
    center: ['30%', '50%'],
    radius: ['40%', '55%'],
    data: [
      {
        value: 55,
        name: '北京'
      }, {
        value: 20,
        name: '武汉'
      }, {
        value: 10,
        name: '杭州'
      }, {
        value: 20,
        name: '广州'
      }, {
        value: 38,
        name: '上海'
      }
    ],
    itemStyle: {
      emphasis: {
        shadowBlur: 10,
        shadowOffsetX: 0,
        shadowColor: 'rgba(0, 2, 2, 0.3)'
      }
    }
  }]
};
const lineOptions = {
  backgroundColor: "#ff764c",
  // title: {
  //   text: '共计800人次',
  //   left: 'left',
  //   itemStyle: {
  //     normal: {
  //       color: '#fff'
  //     }
  //   }
  // },
  color: ["#fff", "#67E0E3", "#9FE6B8"],
  tooltip: {
    top: '20',
    left: '20',
    right: '20',
    bottom: '20',
    show: true,
    trigger: 'axis'
  },
  xAxis: {
    type: 'category',
    boundaryGap: false,
    data: ['周一', '周二', '周三', '周四', '周五', '周六', '周日'],
    axisLine: {
      lineStyle: {
        color: '#fff'
      }
    }
    // show: false
  },
  yAxis: {
    x: 'center',
    type: 'value',
    splitLine: {
      lineStyle: {
        type: 'dashed'
      }
    },
    axisLine: {
      lineStyle: {
        color: '#fff'
      }
    }
    // show: false
  },
  series: [
    {
      name: 'A',
      type: 'line',
      smooth: true,
      data: [18, 36, 65, 30, 78, 40, 33]
    }
  ]
};
export default {
  data () {
    return {
      indexPicker: 3,
      array: ['全部', '近七天', '近三个月', '近半年', '近一年'],
      selectTime: '',
      userInfo: {}, // 用户信息
      ec: {
        options: options
      },
      ecLine: {
        options: lineOptions
      }
    }
  },
  mounted () {
    this.checkTime()

  },
  onShow () {
  },
  watch: {
    // ..
  },
  created () {
    // 调用应用实例的方法获取全局数据
    this.getUserInfo()
  },
  methods: {
    getUserInfo () {
      // 调用登录接口
      wx.login({
        success: () => {
          wx.getUserInfo({
            success: (res) => {
              this.userInfo = res.userInfo
            }
          })
        }
      })
    },
    // 选择时间
    PickerChange(e) {
      this.selectTime = this.array[e.mp.detail.value]
      this.array.forEach( (item, index) => {
        if(this.array[e.mp.detail.value] === item) {
          this.indexPicker = index
        }
      })
    },
    checkTime() {
       this.array.forEach( (item, index) => {
        if(this.indexPicker === index) {
          this.selectTime = this.array[index]
        }
      })
    }
  }
}
</script>

<style lang="scss" scoped>
// .userinfo {
//   display: flex;
//   flex-direction: column;
//   align-items: center;
// }
// 标题title
@mixin diseeaseInfo {
  height: 78rpx;
  line-height: 78rpx;
  color: #666;
  font-weight: bold;
  .title {
    float: left;
    text-indent: 20rpx;
    font-size: 30rpx;
  }
  .select {
    float: right;
    font-size: 28rpx;
    padding-right: 2em;
    span {
      font-weight: normal;
    }
  }
}
// 宽高
@mixin size($width, $height, $background) {
  width: $width;
  height: $height;
  background: $background;
}
.header {
  margin-bottom: 10rpx;
  padding: 30rpx 24rpx;
  width: 706rpx;
  height: 406rpx;
  background: #f8f8f8;
  .header-info {
    height: 162rpx;
    margin: 10rpx 0 42rpx;
    img {
      width: 162rpx;
      height: 162rpx;
      float: left;
      border-radius: 50%;
    }
    p{
      line-height: 46rpx;
      width: 420rpx;
      font-size: 30rpx;
      color: #818181;
      padding-left: 33rpx;
      float: left;
      text-indent:2em;
    }
    .search {
      float:right;
      width:30rpx;
      margin-right:34rpx;
    }
  }
  .header-message {

    padding: 28rpx 20rpx;
    width: 666rpx;
    background: #fff;
    box-shadow:0 0 10rpx rgba(0, 0, 0, 0.25);
    display:flex;
    border-radius:10rpx;
    .message {
      flex: 1;
      height: 120rpx;
      border-left: 1rpx solid #e5e5e5;
      color: #6c6c6c;
      text-align: center;
      p {
        font-size: 26rpx;
        line-height:50rpx;
        .number {
          font-size:38rpx;
          color:#333;
        }
        .name {
          color:#f36800;
        }
      }
    }
    .message:nth-of-type(1) {
      border: none;
    }
  }
}
.pie-content {
  margin-bottom: 20rpx;
  @include size(100%, 380rpx, #fff);
  .diseease-info {
    @include diseeaseInfo;
  }
  .disease-echart, .medicine-echart {
    @include size(100%, 300rpx, #fff);
    .canvas {
      @include size(100%, 300rpx, #fff);
    }
  }
}
.line-content {
  margin-bottom: 20rpx;
  @include size(100%, 510rpx, #fff);
  .diseease-info {
    @include diseeaseInfo;
  }
  .medicine-echart {
    @include size(92%, 400rpx, #fff);
    margin: 2% 4%;
    border-radius: 3%;
    overflow: hidden;
    .canvas {
      @include size(100%, 100%, #fff);
    }
  }
}
.care-content {
  margin-bottom: 20rpx;
  @include size(100%, 610rpx, #fff);
  .diseease-info {
    @include diseeaseInfo;
  }
  .care-list {
    margin: 2% 4%;
    @include size(92%, 500rpx, #fff);
    overflow-y: auto;
    i-row {
      font-size: 26rpx;
      line-height: 65rpx;
      color:#666;
      height: 65rpx;
      i-col {
        border-bottom: 1rpx solid #666;
      }
    }
  }

}

</style>
