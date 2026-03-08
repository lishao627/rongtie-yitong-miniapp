<template>
  <!-- 优惠券页面：延续小程序设计风格 -->
  <view class="page">
    <!-- 状态栏占位 -->
    <view class="status-bar-placeholder"></view>
    
    <!-- 顶部导航栏 -->
    <view class="header">
      <view class="header-left" @tap="goBack">
        <text class="header-icon">←</text>
      </view>
      <text class="header-title">我的优惠券</text>
      <view class="header-right"></view>
    </view>

    <!-- 优惠券状态筛选 -->
    <view class="filter-bar">
      <view 
        v-for="filter in filters" 
        :key="filter.value"
        class="filter-item"
        :class="{ active: currentFilter === filter.value }"
        @tap="switchFilter(filter.value)"
      >
        {{ filter.label }}
      </view>
    </view>

    <!-- 优惠券列表 -->
    <view class="content-wrap">
      <view 
        v-for="(coupon, index) in filteredCoupons" 
        :key="coupon.id"
        class="coupon-item"
        :class="{ 'coupon-used': coupon.status === 'used', 'coupon-expired': coupon.status === 'expired' }"
        :style="{ animationDelay: (index * 0.1) + 's' }"
      >
        <view class="coupon-left">
          <view class="coupon-amount">
            <text class="amount-symbol">¥</text>
            <text class="amount-number">{{ coupon.amount }}</text>
          </view>
          <text class="coupon-condition">满{{ coupon.minSpend }}元可用</text>
        </view>
        <view class="coupon-right">
          <view class="coupon-info">
            <text class="coupon-name">{{ coupon.name }}</text>
            <text class="coupon-desc">{{ coupon.description }}</text>
            <text class="coupon-time">有效期至：{{ coupon.expireTime }}</text>
          </view>
          <view class="coupon-action">
            <button 
              v-if="coupon.status === 'unused'" 
              class="btn-use"
              type="default"
              @tap.stop="useCoupon(coupon)"
            >立即使用</button>
            <text v-else-if="coupon.status === 'used'" class="status-text">已使用</text>
            <text v-else class="status-text">已过期</text>
          </view>
        </view>
      </view>

      <!-- 空优惠券提示 -->
      <view v-if="filteredCoupons.length === 0" class="empty-coupon">
        <text class="empty-icon">优惠券</text>
        <text class="empty-text">暂无{{ currentFilterText }}优惠券</text>
        <button class="btn-go-get" type="default" @tap="goGetCoupon">去领券</button>
      </view>
    </view>

    <!-- 底部领券入口 -->
    <view class="bottom-bar">
      <view class="bottom-content">
        <text class="bottom-text">没有合适的优惠券？</text>
        <button class="btn-get-more" type="default" @tap="goGetCoupon">去领更多</button>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      currentFilter: 'unused',
      filters: [
        { label: '未使用', value: 'unused' },
        { label: '已使用', value: 'used' },
        { label: '已过期', value: 'expired' }
      ],
      coupons: [
        {
          id: '1',
          name: '新用户专享券',
          amount: '10',
          minSpend: '50',
          description: '全场通用，仅限新用户',
          expireTime: '2024-12-31',
          status: 'unused'
        },
        {
          id: '2',
          name: '满减优惠券',
          amount: '20',
          minSpend: '100',
          description: '满100元可用，部分商品除外',
          expireTime: '2024-12-31',
          status: 'unused'
        },
        {
          id: '3',
          name: '水果类专用券',
          amount: '5',
          minSpend: '30',
          description: '仅限水果类商标使用',
          expireTime: '2024-11-30',
          status: 'used'
        },
        {
          id: '4',
          name: '蔬菜类专用券',
          amount: '8',
          minSpend: '40',
          description: '仅限蔬菜类商标使用',
          expireTime: '2024-10-31',
          status: 'expired'
        },
        {
          id: '5',
          name: '定制类优惠券',
          amount: '30',
          minSpend: '200',
          description: '仅限定制类商标使用',
          expireTime: '2024-12-31',
          status: 'unused'
        }
      ]
    }
  },
  computed: {
    filteredCoupons() {
      if (this.currentFilter === 'all') {
        return this.coupons
      }
      return this.coupons.filter(coupon => coupon.status === this.currentFilter)
    },
    currentFilterText() {
      const filter = this.filters.find(f => f.value === this.currentFilter)
      return filter ? filter.label : ''
    }
  },
  methods: {
    goBack() {
      uni.navigateBack()
    },
    switchFilter(filter) {
      this.currentFilter = filter
    },
    useCoupon(coupon) {
      uni.showToast({ title: `使用${coupon.name}`, icon: 'none' })
      // 可以跳转到商品列表页或购物车
      setTimeout(() => {
        uni.switchTab({ url: '/pages/shop/list' })
      }, 1500)
    },
    goGetCoupon() {
      uni.showToast({ title: '领券中心开发中', icon: 'none' })
    }
  }
}
</script>

<style>
.page {
  min-height: 100vh;
  width: 100%;
  padding-bottom: 140rpx;
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
  display: flex;
  flex-direction: column;
}

/* 状态栏占位 */
.status-bar-placeholder {
  height: var(--status-bar-height, 44rpx);
  width: 100%;
  flex-shrink: 0;
}

/* 顶部导航栏 */
.header {
  height: 88rpx;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  padding: 0 36rpx;
  background: rgba(255, 255, 255, 0.95);
  border-bottom: 1rpx solid rgba(0, 0, 0, 0.1);
  box-shadow: 0 2rpx 12rpx rgba(0, 0, 0, 0.08);
  flex-shrink: 0;
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
}

/* 筛选标签 */
.filter-bar {
  margin: 24rpx 36rpx;
  padding: 16rpx;
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  background: rgba(255, 255, 255, 0.95);
  border-radius: 16rpx;
  box-shadow: 0 2rpx 12rpx rgba(0, 0, 0, 0.08);
}

.filter-item {
  padding: 12rpx 32rpx;
  font-size: 28rpx;
  color: #666666;
  border-radius: 28rpx;
  transition: all 0.3s ease;
}

.filter-item.active {
  background: #66bb6a;
  color: #ffffff;
}

/* 优惠券列表 */
.content-wrap {
  padding: 0 36rpx;
}

.coupon-item {
  margin-bottom: 24rpx;
  display: flex;
  flex-direction: row;
  background: rgba(255, 255, 255, 0.95);
  border-radius: 20rpx;
  box-shadow: 0 2rpx 12rpx rgba(0, 0, 0, 0.08);
  overflow: hidden;
  animation: fadeIn 0.5s ease forwards;
  opacity: 0;
}

.coupon-used {
  opacity: 0.6;
}

.coupon-expired {
  opacity: 0.5;
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

.coupon-left {
  width: 200rpx;
  padding: 32rpx 24rpx;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background: linear-gradient(135deg, #66bb6a, #43a047);
  gap: 12rpx;
}

.coupon-used .coupon-left {
  background: linear-gradient(135deg, #999999, #666666);
}

.coupon-expired .coupon-left {
  background: linear-gradient(135deg, #cccccc, #999999);
}

.coupon-amount {
  display: flex;
  align-items: baseline;
  gap: 4rpx;
}

.amount-symbol {
  font-size: 32rpx;
  color: #ffffff;
  font-weight: 600;
}

.amount-number {
  font-size: 56rpx;
  color: #ffffff;
  font-weight: 700;
}

.coupon-condition {
  font-size: 22rpx;
  color: rgba(255, 255, 255, 0.9);
}

.coupon-right {
  flex: 1;
  padding: 24rpx;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  gap: 16rpx;
}

.coupon-info {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 8rpx;
}

.coupon-name {
  font-size: 30rpx;
  font-weight: 600;
  color: #333333;
}

.coupon-desc {
  font-size: 24rpx;
  color: #666666;
  line-height: 1.4;
}

.coupon-time {
  font-size: 22rpx;
  color: #999999;
}

.coupon-action {
  display: flex;
  align-items: center;
}

.btn-use {
  padding: 0 28rpx;
  height: 56rpx;
  line-height: 56rpx;
  font-size: 26rpx;
  color: #ffffff;
  background: linear-gradient(90deg, #66bb6a, #43a047);
  border-radius: 28rpx;
  border: none;
  margin: 0;
}

.btn-use::after {
  border: none;
}

.status-text {
  font-size: 26rpx;
  color: #999999;
  font-weight: 600;
}

/* 空优惠券提示 */
.empty-coupon {
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
  font-size: 30rpx;
  color: #999999;
  margin-bottom: 40rpx;
}

.btn-go-get {
  padding: 0 56rpx;
  height: 72rpx;
  line-height: 72rpx;
  font-size: 30rpx;
  color: #ffffff;
  background: linear-gradient(90deg, #66bb6a, #43a047);
  border-radius: 36rpx;
  border: none;
}

.btn-go-get::after {
  border: none;
}

/* 底部领券入口 */
.bottom-bar {
  position: fixed;
  left: 0;
  right: 0;
  bottom: 0;
  padding: 20rpx 36rpx;
  padding-bottom: calc(20rpx + env(safe-area-inset-bottom));
  background: rgba(255, 255, 255, 0.98);
  border-top: 1rpx solid rgba(0, 0, 0, 0.1);
  box-shadow: 0 -2rpx 12rpx rgba(0, 0, 0, 0.05);
}

.bottom-content {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
}

.bottom-text {
  font-size: 28rpx;
  color: #666666;
}

.btn-get-more {
  padding: 0 32rpx;
  height: 64rpx;
  line-height: 64rpx;
  font-size: 28rpx;
  color: #66bb6a;
  background: rgba(102, 187, 106, 0.1);
  border-radius: 32rpx;
  border: 2rpx solid #66bb6a;
  margin: 0;
}

.btn-get-more::after {
  border: none;
}
</style>