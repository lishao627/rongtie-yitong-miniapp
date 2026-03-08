<template>
  <!-- 订单页面：延续登录页风格，查看订单状态 -->
  <view class="page">
    <!-- 状态栏占位 -->
    <view class="status-bar"></view>
    
    <!-- 顶部导航栏 -->
    <view class="header glass-card">
      <view class="header-left" @tap="goBack">
        <text class="header-icon">←</text>
      </view>
      <text class="header-title">我的订单</text>
      <view class="header-right"></view>
    </view>

    <!-- 订单状态筛选 -->
    <view class="filter-bar glass-card">
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

    <!-- 订单列表 -->
    <view class="content-wrap">
      <!-- 订单卡片 -->
      <view 
        v-for="(order, index) in filteredOrders" 
        :key="order.id"
        class="glass-card order-item animate-fade-in"
        :style="{ animationDelay: (index * 0.1) + 's' }"
        @tap="goOrderDetail(order.id)"
      >
        <view class="order-header">
          <text class="order-id">订单号：{{ order.id }}</text>
          <text class="order-status" :class="order.statusClass">{{ order.statusText }}</text>
        </view>
        <view class="order-content">
          <view 
            v-for="(item, itemIndex) in order.items" 
            :key="itemIndex"
            class="order-product"
          >
            <view class="product-image">
              <image :src="item.image" mode="aspectFill" />
            </view>
            <view class="product-info">
              <view class="product-name">{{ item.name }}</view>
              <view class="product-spec">{{ item.size }} / {{ item.material }}</view>
              <view class="product-price">¥{{ item.price }} × {{ item.quantity }}</view>
            </view>
          </view>
        </view>
        <view class="order-footer">
          <text class="order-total">共{{ order.totalItems }}件商品 合计：¥{{ order.totalPrice }}</text>
          <view class="order-actions">
            <button 
              v-if="order.status === 'pending'" 
              class="btn-pay" 
              type="default" 
              @tap.stop="onPay(order.id)"
            >立即支付</button>
            <button 
              v-if="order.status === 'pending'" 
              class="btn-cancel" 
              type="default" 
              @tap.stop="onCancel(order.id)"
            >取消订单</button>
            <button 
              v-if="order.status === 'delivered'" 
              class="btn-confirm" 
              type="default" 
              @tap.stop="onConfirm(order.id)"
            >确认收货</button>
            <button 
              v-if="order.status === 'completed'" 
              class="btn-review" 
              type="default" 
              @tap.stop="onReview(order.id)"
            >评价</button>
          </view>
        </view>
      </view>

      <!-- 空订单提示 -->
      <view v-if="filteredOrders.length === 0" class="empty-order">
        <text class="empty-text">暂无{{ currentFilterText }}订单</text>
        <button class="btn-go-shop" type="default" @tap="goShop">去购物</button>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      currentFilter: 'all',
      filters: [
        { label: '全部', value: 'all' },
        { label: '待付款', value: 'pending' },
        { label: '待发货', value: 'shipping' },
        { label: '待收货', value: 'delivered' },
        { label: '已完成', value: 'completed' }
      ],
      orders: [
        {
          id: '202401010001',
          status: 'pending',
          statusText: '待付款',
          statusClass: 'status-pending',
          totalItems: 2,
          totalPrice: '48.8',
          items: [
            {
              name: '苹果可溶性商标',
              price: '19.9',
              quantity: 1,
              size: '中尺寸',
              material: '标准材质',
              image: '/static/apple.jpg'
            },
            {
              name: '草莓可溶性商标',
              price: '28.9',
              quantity: 1,
              size: '中尺寸',
              material: '标准材质',
              image: '/static/草莓.jpg'
            }
          ]
        },
        {
          id: '202401020002',
          status: 'shipping',
          statusText: '待发货',
          statusClass: 'status-shipping',
          totalItems: 1,
          totalPrice: '21.9',
          items: [
            {
              name: '梨可溶性商标',
              price: '21.9',
              quantity: 1,
              size: '中尺寸',
              material: '标准材质',
              image: '/static/梨子.jpg'
            }
          ]
        },
        {
          id: '202401030003',
          status: 'delivered',
          statusText: '待收货',
          statusClass: 'status-delivered',
          totalItems: 1,
          totalPrice: '26.9',
          items: [
            {
              name: '葡萄可溶性商标',
              price: '26.9',
              quantity: 1,
              size: '中尺寸',
              material: '标准材质',
              image: '/static/grape.jpg'
            }
          ]
        },
        {
          id: '202401040004',
          status: 'completed',
          statusText: '已完成',
          statusClass: 'status-completed',
          totalItems: 1,
          totalPrice: '32.9',
          items: [
            {
              name: '蓝莓可溶性商标',
              price: '32.9',
              quantity: 1,
              size: '中尺寸',
              material: '标准材质',
              image: '/static/蓝莓.jpg'
            }
          ]
        }
      ]
    }
  },
  computed: {
    filteredOrders() {
      if (this.currentFilter === 'all') {
        return this.orders
      }
      return this.orders.filter(order => order.status === this.currentFilter)
    },
    currentFilterText() {
      const filter = this.filters.find(f => f.value === this.currentFilter)
      return filter ? filter.label : ''
    }
  },
  onLoad(options) {
    // 接收状态参数，如果有则设置当前筛选状态
    if (options.status) {
      const validStatuses = ['all', 'pending', 'shipping', 'delivered', 'completed']
      if (validStatuses.includes(options.status)) {
        this.currentFilter = options.status
      }
    }
  },
  onShow() {
    // 从缓存读取筛选状态
    const filter = uni.getStorageSync('orderFilter')
    if (filter) {
      const validStatuses = ['all', 'pending', 'shipping', 'delivered', 'completed']
      if (validStatuses.includes(filter)) {
        this.currentFilter = filter
      }
      // 读取后清除缓存
      uni.removeStorageSync('orderFilter')
    }
  },
  methods: {
    goBack() {
      uni.navigateBack()
    },
    switchFilter(filter) {
      this.currentFilter = filter
    },
    goOrderDetail(id) {
      uni.navigateTo({ url: '/pages/orders/detail?id=' + id })
    },
    onPay(id) {
      const order = this.orders.find(o => o.id === id)
      if (order) {
        uni.navigateTo({ 
          url: '/pages/orders/payment?id=' + id + '&price=' + order.totalPrice 
        })
      }
    },
    onCancel(id) {
      uni.showModal({
        title: '提示',
        content: '确定要取消订单吗？',
        success: (res) => {
          if (res.confirm) {
            uni.showToast({ title: '订单已取消', icon: 'success' })
          }
        }
      })
    },
    onConfirm(id) {
      uni.showToast({ title: '已确认收货', icon: 'success' })
    },
    onReview(id) {
      uni.navigateTo({ url: '/pages/orders/review?id=' + id })
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

/* 状态栏占位 */
.status-bar {
  height: var(--status-bar-height);
  width: 100%;
}

/* 顶部导航栏 */
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
  background: rgba(255, 255, 255, 0.95);
  border-bottom: 1rpx solid rgba(0, 0, 0, 0.1);
  box-shadow: 0 2rpx 12rpx rgba(0, 0, 0, 0.08);
}

.header-left {
  width: 60rpx;
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
  width: 60rpx;
}

/* 订单状态筛选 */
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
  padding: 12rpx 24rpx;
  font-size: 28rpx;
  color: #666666;
  border-radius: 28rpx;
  transition: all 0.3s ease;
}

.filter-item.active {
  background: #66bb6a;
  color: #ffffff;
}

/* 内容区 */
.content-wrap {
  padding: 0 36rpx;
}

/* 订单卡片 */
.order-item {
  margin-bottom: 24rpx;
  padding: 24rpx;
  background: rgba(255, 255, 255, 0.95);
  border-radius: 20rpx;
  box-shadow: 0 2rpx 12rpx rgba(0, 0, 0, 0.08);
}

.order-header {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 20rpx;
  padding-bottom: 20rpx;
  border-bottom: 1rpx solid rgba(0, 0, 0, 0.06);
}

.order-id {
  font-size: 26rpx;
  color: #999999;
}

.order-status {
  font-size: 28rpx;
  font-weight: 600;
}

.status-pending {
  color: #ff6b6b;
}

.status-shipping {
  color: #f5a623;
}

.status-delivered {
  color: #66bb6a;
}

.status-completed {
  color: #666666;
}

.order-content {
  margin-bottom: 20rpx;
}

.order-product {
  display: flex;
  flex-direction: row;
  align-items: center;
  gap: 20rpx;
  margin-bottom: 16rpx;
}

.order-product:last-child {
  margin-bottom: 0;
}

.product-image {
  width: 120rpx;
  height: 120rpx;
  border-radius: 12rpx;
  overflow: hidden;
  background: rgba(0, 0, 0, 0.05);
  flex-shrink: 0;
}

.product-image image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.product-info {
  flex: 1;
}

.product-name {
  font-size: 30rpx;
  font-weight: 600;
  color: #333333;
  margin-bottom: 8rpx;
}

.product-spec {
  font-size: 24rpx;
  color: #999999;
  margin-bottom: 8rpx;
}

.product-price {
  font-size: 28rpx;
  color: #66bb6a;
  font-weight: 600;
}

.order-footer {
  padding-top: 20rpx;
  border-top: 1rpx solid rgba(0, 0, 0, 0.06);
}

.order-total {
  font-size: 28rpx;
  color: #666666;
  display: block;
  margin-bottom: 16rpx;
  text-align: right;
}

.order-actions {
  display: flex;
  flex-direction: row;
  justify-content: flex-end;
  gap: 16rpx;
}

.order-actions button {
  margin: 0;
  padding: 0 32rpx;
  height: 64rpx;
  line-height: 64rpx;
  font-size: 28rpx;
  border-radius: 32rpx;
  border: none;
}

.order-actions button::after {
  border: none;
}

.btn-pay {
  background: linear-gradient(90deg, #66bb6a, #43a047);
  color: #ffffff;
}

.btn-cancel {
  background: rgba(0, 0, 0, 0.06);
  color: #666666;
}

.btn-confirm {
  background: linear-gradient(90deg, #66bb6a, #43a047);
  color: #ffffff;
}

.btn-review {
  background: rgba(102, 187, 106, 0.1);
  color: #66bb6a;
  border: 1rpx solid #66bb6a;
}

/* 空订单 */
.empty-order {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 120rpx 36rpx;
}

.empty-text {
  font-size: 32rpx;
  color: #999999;
  margin-bottom: 40rpx;
}

.btn-go-shop {
  padding: 0 56rpx;
  height: 80rpx;
  line-height: 80rpx;
  font-size: 30rpx;
  color: #ffffff;
  background: linear-gradient(90deg, #66bb6a, #43a047);
  border-radius: 40rpx;
  border: none;
}

.btn-go-shop::after {
  border: none;
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