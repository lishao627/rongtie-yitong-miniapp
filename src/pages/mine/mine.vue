<template>
  <!-- 我的页面：延续登录页风格，用户信息管理 -->
  <view class="page">
    <!-- 状态栏占位 -->
    <view class="status-bar"></view>
    
    <!-- 顶部导航栏 -->
    <view class="header glass-card">
      <view class="header-left header-content"></view>
      <text class="header-title header-content">我的</text>
      <view class="header-right header-content" @tap="onSettings">
        <text class="header-icon">设置</text>
      </view>
    </view>

    <!-- 用户信息区域 -->
    <view class="user-section glass-card animate-fade-in" @tap="goProfile">
      <view class="user-avatar glass-card-content">
        <text class="avatar-text">R</text>
      </view>
      <view class="user-info glass-card-content">
        <view class="user-name">溶贴易通用户</view>
        <view class="user-level">普通会员</view>
      </view>
      <view class="user-points glass-card-content">
        <text class="points-text">积分：128</text>
      </view>
    </view>

    <!-- 订单状态 -->
    <view class="order-section glass-card animate-fade-in" style="animation-delay: 0.1s;">
      <view class="section-header glass-card-content">
        <text class="section-title">我的订单</text>
        <text class="section-more" @tap="goOrders">全部</text>
      </view>
      <view class="order-status glass-card-content">
        <view class="order-item" @tap="goOrderList('pending')">
          <text class="order-text">待付款</text>
        </view>
        <view class="order-item" @tap="goOrderList('shipping')">
          <text class="order-text">待发货</text>
        </view>
        <view class="order-item" @tap="goOrderList('delivered')">
          <text class="order-text">待收货</text>
        </view>
        <view class="order-item" @tap="goOrderList('completed')">
          <text class="order-text">待评价</text>
        </view>
      </view>
    </view>

    <!-- 功能菜单 -->
    <view class="menu-section glass-card animate-fade-in" style="animation-delay: 0.2s;">
      <view class="menu-item glass-card-content" @tap="goAddress">
        <text class="menu-icon">地址</text>
        <text class="menu-text">收货地址</text>
      </view>
      <view class="menu-item glass-card-content" @tap="goCoupons">
        <text class="menu-icon">优惠</text>
        <text class="menu-text">优惠券</text>
      </view>
      <view class="menu-item glass-card-content" @tap="goFavorites">
        <text class="menu-icon">收藏</text>
        <text class="menu-text">我的收藏</text>
      </view>
      <view class="menu-item glass-card-content" @tap="goHistory">
        <text class="menu-icon">历史</text>
        <text class="menu-text">浏览历史</text>
      </view>
    </view>

    <!-- 服务支持 -->
    <view class="service-section glass-card animate-fade-in" style="animation-delay: 0.3s;">
      <view class="menu-item glass-card-content" @tap="goCustomerService">
        <text class="menu-icon">客服</text>
        <text class="menu-text">客服中心</text>
      </view>
      <view class="menu-item glass-card-content" @tap="goFAQ">
        <text class="menu-icon">问题</text>
        <text class="menu-text">常见问题</text>
      </view>
      <view class="menu-item glass-card-content" @tap="goAbout">
        <text class="menu-icon">关于</text>
        <text class="menu-text">关于我们</text>
      </view>
    </view>

    <!-- 底部版本信息 -->
    <view class="version-info">
      <text class="version-text">溶贴易通 v1.0.0</text>
    </view>
  </view>
</template>

<script>
export default {
  methods: {
    onSettings() {
      uni.showToast({ title: '设置功能开发中', icon: 'none' })
    },
    goOrders() {
      uni.setStorageSync('orderFilter', 'all')
      uni.switchTab({ url: '/pages/orders/orders' })
    },
    goOrderList(status) {
      uni.setStorageSync('orderFilter', status)
      uni.switchTab({ url: '/pages/orders/orders' })
    },
    goAddress() {
      uni.navigateTo({ url: '/pages/address/address' })
    },
    goCoupons() {
      uni.navigateTo({ url: '/pages/coupons/coupons' })
    },
    goFavorites() {
      uni.navigateTo({ url: '/pages/favorites/favorites' })
    },
    goHistory() {
      uni.navigateTo({ url: '/pages/history/history' })
    },
    goCustomerService() {
      uni.navigateTo({ url: '/pages/service/service' })
    },
    goFAQ() {
      uni.navigateTo({ url: '/pages/faq/faq' })
    },
    goAbout() {
      uni.navigateTo({ url: '/pages/about/about' })
    },
    goProfile() {
      uni.navigateTo({ url: '/pages/profile/profile' })
    }
  }
}
</script>

<style>
.page {
  min-height: 100vh;
  width: 100%;
  padding-bottom: 60rpx;
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
/* 状态栏占位 */
.status-bar {
  height: var(--status-bar-height);
  width: 100%;
}

.header {
  position: sticky;
  top: 0;
  z-index: 100;
  height: 100rpx;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  padding: 0 36rpx;
  position: relative;
  overflow: hidden;
}

.header::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.8), rgba(255, 255, 255, 0.2));
  border-bottom: 1rpx solid rgba(255, 255, 255, 0.5);
  backdrop-filter: blur(12px);
  box-shadow: 0 2rpx 12rpx rgba(0, 0, 0, 0.04), 0 4rpx 20rpx rgba(0, 0, 0, 0.03);
  z-index: 0;
}

.header-content {
  position: relative;
  z-index: 1;
}
.header-left,
.header-right {
  width: 80rpx;
  height: 100rpx;
  display: flex;
  align-items: center;
  justify-content: center;
}
.header-icon {
  font-size: 28rpx;
  color: #666666;
}
.header-title {
  font-size: 32rpx;
  font-weight: 600;
  color: #666666;
}

/* 统一玻璃卡片样式 */
.glass-card {
  position: relative;
  overflow: hidden;
  border-radius: 20rpx;
}

.glass-card::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.8), rgba(255, 255, 255, 0.2));
  border: 1rpx solid rgba(255, 255, 255, 0.5);
  backdrop-filter: blur(12px);
  border-radius: 20rpx;
  z-index: 0;
}

.glass-card-content {
  position: relative;
  z-index: 1;
}

/* 用户信息区域 */
.user-section {
  margin: 24rpx 36rpx;
  padding: 40rpx 36rpx;
  display: flex;
  flex-direction: row;
  align-items: center;
  gap: 32rpx;
}

.user-avatar {
  width: 140rpx;
  height: 140rpx;
  border-radius: 70rpx;
  overflow: hidden;
  background: rgba(255, 255, 255, 0.9);
  border: 2rpx solid rgba(102, 187, 106, 0.3);
  display: flex;
  align-items: center;
  justify-content: center;
}

.avatar-text {
  font-size: 72rpx;
  font-weight: 700;
  color: #66bb6a;
}

.user-info {
  flex: 1;
  min-width: 0;
}

.user-name {
  font-size: 36rpx;
  font-weight: 600;
  color: #666666;
  margin-bottom: 8rpx;
}

.user-level {
  font-size: 24rpx;
  color: #999999;
  background: rgba(102, 187, 106, 0.1);
  padding: 4rpx 16rpx;
  border-radius: 12rpx;
  display: inline-block;
}

.user-points {
  display: flex;
  align-items: center;
}

.points-text {
  font-size: 24rpx;
  color: #66bb6a;
  background: rgba(102, 187, 106, 0.1);
  padding: 8rpx 16rpx;
  border-radius: 16rpx;
}

/* 订单状态 */
.order-section {
  margin: 0 36rpx 24rpx;
  padding: 32rpx 36rpx;
}

.section-header {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 32rpx;
}

.section-title {
  font-size: 32rpx;
  font-weight: 600;
  color: #666666;
}

.section-more {
  font-size: 32rpx;
  color: #66bb6a;
  font-weight: 700;
}

.order-status {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-around;
}

.order-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 12rpx;
}

.order-text {
  font-size: 28rpx;
  color: #666666;
}

/* 功能菜单 */
.menu-section,
.service-section {
  margin: 0 36rpx 24rpx;
  padding: 0;
  overflow: hidden;
}

.menu-item {
  display: flex;
  flex-direction: row;
  align-items: center;
  padding: 32rpx 36rpx;
  border-bottom: 1rpx solid rgba(0, 0, 0, 0.06);
  transition: all 0.2s ease;
}

.menu-item:active {
  background: rgba(0, 0, 0, 0.03);
}

.menu-item:last-child {
  border-bottom: none;
}

.menu-icon {
  font-size: 28rpx;
  margin-right: 24rpx;
  width: 80rpx;
  text-align: left;
  font-weight: 600;
  color: #666666;
}

.menu-text {
  flex: 1;
  font-size: 28rpx;
  color: #666666;
}

.menu-arrow {
  font-size: 28rpx;
  color: #999999;
  font-weight: bold;
}

/* 底部版本信息 */
.version-info {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top: 40rpx;
  margin-bottom: 60rpx;
}

.version-text {
  font-size: 22rpx;
  color: #999999;
}

/* 动画 */
.animate-fade-in {
  animation: fadeIn 0.5s ease forwards;
  opacity: 0;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(20rpx);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>