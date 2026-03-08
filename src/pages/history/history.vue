<template>
  <!-- 浏览历史页面：延续小程序设计风格 -->
  <view class="page">
    <!-- 顶部导航栏 -->
    <view class="header">
      <view class="header-left" @tap="goBack">
        <text class="header-icon">←</text>
      </view>
      <text class="header-title">浏览历史</text>
      <view class="header-right" @tap="clearHistory">
        <text class="header-action">清空</text>
      </view>
    </view>

    <!-- 历史列表 -->
    <view class="content-wrap">
      <view 
        v-for="(item, index) in history" 
        :key="item.id"
        class="history-item"
        @tap="goProductDetail(item.id)"
      >
        <view class="history-image">
          <image :src="item.image" mode="aspectFill" />
        </view>
        <view class="history-info">
          <text class="history-name">{{ item.name }}</text>
          <text class="history-desc">{{ item.description }}</text>
          <view class="history-bottom">
            <text class="history-price">¥{{ item.price }}</text>
            <text class="history-time">{{ item.viewTime }}</text>
          </view>
        </view>
      </view>

      <!-- 空历史提示 -->
      <view v-if="history.length === 0" class="empty-history">
        <text class="empty-icon">历史</text>
        <text class="empty-text">暂无浏览记录</text>
        <text class="empty-hint">浏览过的商品会出现在这里</text>
        <button class="btn-go-shop" type="default" @tap="goShop">去逛逛</button>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      history: [
        {
          id: '1',
          name: '苹果可溶性商标',
          description: '食品级安全，环保可降解',
          price: '19.9',
          image: '/static/apple.jpg',
          viewTime: '今天 10:23'
        },
        {
          id: '2',
          name: '草莓可溶性商标',
          description: '食品级安全，环保可降解',
          price: '28.9',
          image: '/static/草莓.jpg',
          viewTime: '今天 09:15'
        },
        {
          id: '3',
          name: '定制水果商标',
          description: '根据客户需求定制，食品级安全',
          price: '29.9',
          image: '/static/定制水果.jpg',
          viewTime: '昨天 16:42'
        },
        {
          id: '4',
          name: '葡萄可溶性商标',
          description: '食品级安全，环保可降解',
          price: '26.9',
          image: '/static/grape.jpg',
          viewTime: '昨天 14:30'
        }
      ]
    }
  },
  methods: {
    goBack() {
      uni.navigateBack()
    },
    clearHistory() {
      if (this.history.length === 0) {
        uni.showToast({ title: '暂无浏览记录', icon: 'none' })
        return
      }
      uni.showModal({
        title: '提示',
        content: '确定要清空浏览历史吗？',
        success: (res) => {
          if (res.confirm) {
            this.history = []
            uni.showToast({ title: '已清空', icon: 'success' })
          }
        }
      })
    },
    goProductDetail(id) {
      uni.navigateTo({ url: '/pages/product/detail?id=' + id })
    },
    goShop() {
      uni.switchTab({ url: '/pages/index/index' })
    }
  }
}
</script>

<style>
.page {
  min-height: 100vh;
  width: 100%;
  padding-bottom: 40rpx;
  box-sizing: border-box;
  background-image: radial-gradient(
    circle at 10% 0,
    #e8f5e9 0,
    #c8e6c9 40%,
    #a5d6a7 70%,
    #81c784 100%
  );
  background-repeat: no-repeat;
  background-size: cover;
}

/* 顶部导航栏 */
.header {
  height: 100rpx;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  padding: 0 36rpx;
  background: rgba(255, 255, 255, 0.95);
  border-bottom: 1rpx solid rgba(0, 0, 0, 0.1);
  box-shadow: 0 2rpx 12rpx rgba(0, 0, 0, 0.08);
}

.header-left {
  width: 80rpx;
  height: 60rpx;
  display: flex;
  align-items: center;
  justify-content: center;
}

.header-icon {
  font-size: 36rpx;
  color: #666666;
}

.header-title {
  font-size: 34rpx;
  font-weight: 600;
  color: #333333;
}

.header-right {
  width: 80rpx;
  display: flex;
  align-items: center;
  justify-content: flex-end;
}

.header-action {
  font-size: 28rpx;
  color: #66bb6a;
}

/* 历史列表 */
.content-wrap {
  padding: 24rpx 36rpx;
}

.history-item {
  margin-bottom: 24rpx;
  padding: 24rpx;
  display: flex;
  flex-direction: row;
  gap: 24rpx;
  background: rgba(255, 255, 255, 0.95);
  border-radius: 20rpx;
  box-shadow: 0 2rpx 12rpx rgba(0, 0, 0, 0.08);
}

.history-image {
  width: 160rpx;
  height: 160rpx;
  border-radius: 12rpx;
  overflow: hidden;
  flex-shrink: 0;
}

.history-image image {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.history-info {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 12rpx;
}

.history-name {
  font-size: 30rpx;
  font-weight: 600;
  color: #333333;
}

.history-desc {
  font-size: 26rpx;
  color: #666666;
  line-height: 1.4;
}

.history-bottom {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  margin-top: auto;
}

.history-price {
  font-size: 32rpx;
  color: #66bb6a;
  font-weight: 700;
}

.history-time {
  font-size: 24rpx;
  color: #999999;
}

/* 空历史提示 */
.empty-history {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 120rpx 36rpx;
}

.empty-icon {
  font-size: 80rpx;
  color: #cccccc;
  margin-bottom: 24rpx;
}

.empty-text {
  font-size: 32rpx;
  color: #666666;
  margin-bottom: 16rpx;
}

.empty-hint {
  font-size: 26rpx;
  color: #999999;
  margin-bottom: 40rpx;
}

.btn-go-shop {
  padding: 0 56rpx;
  height: 72rpx;
  line-height: 72rpx;
  font-size: 30rpx;
  color: #ffffff;
  background: linear-gradient(90deg, #66bb6a, #43a047);
  border-radius: 36rpx;
  border: none;
}

.btn-go-shop::after {
  border: none;
}
</style>