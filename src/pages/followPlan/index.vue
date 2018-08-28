<template>
  <div class="followPlan">
    <div class="fixTop">
      <input type="text" placeholder="请输入患者姓名或者疾病名称">
      <div class="search-button">{{searchBtnText}}</div>
      <i-icon type="search" class="search-icon" />
    </div>
    <div class="list">
      <scroll-view scroll-y
        @scrolltoupper="upper"
        @scrolltolower="lower"
        @scroll="scroll"
        :scroll-into-view="toView"
        :scroll-top="scrollTop"
      >
        <i-tabs :current="current" color="#f759ab" @change="handleChange">
          <i-tab key="1" title="待审核"></i-tab>
          <i-tab key="2" title="已通过"></i-tab>
          <i-tab key="3" title="未通过"></i-tab>
        </i-tabs>
        <div class="tab-container">
          <div class="list-card" v-for="(item, index) in 10" :key="index">
            <div class="title">
              <div class="radios">
               <radio :value="name" :checked="checked" color="#2580c3" @change="handleAnimalChange"/>
              </div>
              <div class="info">
                <span class="brxm">张三丰</span>
                <span class="brxb">男/50岁</span>
                <span class="look-details">查看详情<i-icon type="enter" /></span>
              </div>
            </div>
            <p><span>疾病诊断:</span> 糖尿病,心脏病的早期症状,ganyan,weiyan</p>
            <p><span>随访方案</span>: 糖尿病方案</p>
            <p><span>计划开始日期</span>: 2017-11-06</p>
            <p class="time"><span>23：59:59</span>后自动通过审核</p>
          </div>
        </div>
      </scroll-view>
    </div>
    <div class="fixBottom">
      <div class="fixBottom-left">
        <div class="radio-container">
          <i-radio :value="animal" :checked="checked" @change="handleAnimalChange" />
        </div>
        <div class="radio-number">条数({{checkNumber}}/{{checkTotal}})</div>
      </div>
      <div class="fixBottom-right">通过</div>
    </div>
  </div>
</template>
<script>
  export default {
    name: 'followPlan',
    data () {
      return {
        searchBtnText: '查询',
        animal: '全选',
        checked: false,
        checkNumber: 5,
        checkTotal: 20,
        toView: 'red',
        scrollTop: '20',
        current: '1',
        name: '站三丰'
      }
    },
    mounted () {
    },
    methods: {
      // 全选按钮
      handleAnimalChange(e) {
        this.checked = e.mp.detail.current;
        console.log(e.target.value,e.mp.detail.current);
      },
      upper(e) {
        // console.log(e, 'upper')
        // alert('赏花到顶')
      },
      lower(e) {
        // console.log(e, 'lower')
      },
      scroll(e) {
      },
      // 切换tabs
      handleChange(e) {
        console.log(e);
        this.current = e.target.key

      }
    }
  }
</script>
<style lang="scss">
 @import '~@/assets/styles/mixins.scss';
  .followPlan {
    position: relative;
    overflow: hidden;
    @include size(100vw, 100vh, #eee);
    font-size: 24rpx;
    .fixTop {
      position: absolute;
      top: 0;
      left: 0;
      @include size(718rpx, 60rpx, #eee);
      padding: 16rpx;
      input {
        float: left;
        @include size(563rpx, 54rpx, #fff);
        border-radius:5rpx;
        padding-left: 48rpx;
      }
      .search-button {
        float: left;
        padding:0;
        @include size(100rpx, 54rpx, #eee);
        line-height:54rpx;
        font-size:24rpx;
        text-align: center;
      }
      .search-icon {
        position: absolute;
        top:24rpx;
        left:29rpx;
        z-index:100;

      }
    }
    .fixBottom {
      position: absolute;
      bottom: 0;
      left: 0;
      display: flex;
      @include size(100%, 92rpx, #fff);
      .fixBottom-left {
        flex: 1;
        padding-left: 10rpx;
        justify-content: flex-start;
        align-items: center;
        .radio-container {
          float: left;
          width: 180rpx;
        }
        .radio-number {
          float: left;
          line-height: 92rpx;
        }
      }
      .fixBottom-right {
        flex: 1;
        text-align:center;
        background:#f36837;
        line-height:92rpx;
        height:92rpx;
        color:#fff;
        font-size:32rpx;
      }
    }
    .list {
      margin-top: 92rpx;
      @include size(100vw, 100vh, #eee);
      scroll-view {
        width: 100%;
        height: 100%;
        .tab-container{
          @include size(100vw, 90vh,#eee);
          padding-bottom:180rpx;
        }
      }
    }
  }
  .list-card {
    margin-bottom:20rpx;
    padding:40rpx 20rpx 0rpx;
    @include size(710rpx, 288rpx, #fff);
    .title {
      @include size(710rpx, 60rpx, #fff);
      .radios {
        float: left;
        width: 72rpx;
      }
      .info {
        float: left;
        line-height: 43rpx;
        .brxm {
          font-size:34rpx;
          font-weight:bold;
          color:#555;
        }
        .brxb {
          padding-left:24rpx;
          color:#999;
        }
        .look-details {
          padding-left:284rpx;
          color:#f68300;
        }
      }
    }
    p {
      padding-left:71rpx;
      line-height: 50rpx;
      font-size: 26rpx;
      color:#666;
      span {
        color: #333;
      }
    }
    .time {
      line-height: 60rpx;
      font-size: 32rpx;
      color:#666;
      span {
        padding-right:28rpx;
        color: #2580c3;
      }
    }

  }
</style>