<template>
  <!-- 订单详情页面：延续登录页风格，查看订单详细信息 -->
  <view class="page">
    <!-- 顶部导航栏 -->
    <view class="header glass-card">
      <view class="header-left" @tap="goBack">
        <text class="header-icon">←</text>
      </view>
      <text class="header-title">订单详情</text>
      <view class="header-right"></view>
    </view>

    <!-- 订单状态 -->
    <view class="status-section glass-card">
      <view class="status-icon" :class="order.statusClass">{{ order.statusIcon }}</view>
      <view class="status-text">{{ order.statusText }}</view>
      <view class="status-desc">{{ order.statusDesc }}</view>
    </view>

    <!-- 订单信息 -->
    <view class="info-section glass-card">
      <view class="info-item">
        <text class="info-label">订单号</text>
        <text class="info-value">{{ order.id }}</text>
      </view>
      <view class="info-item">
        <text class="info-label">下单时间</text>
        <text class="info-value">{{ order.createTime }}</text>
      </view>
      <view class="info-item">
        <text class="info-label">支付时间</text>
        <text class="info-value">{{ order.payTime || '未支付' }}</text>
      </view>
      <view class="info-item">
        <text class="info-label">发货时间</text>
        <text class="info-value">{{ order.shipTime || '未发货' }}</text>
      </view>
      <view class="info-item">
        <text class="info-label">收货时间</text>
        <text class="info-value">{{ order.receiveTime || '未收货' }}</text>
      </view>
    </view>

    <!-- 收货信息 -->
    <view class="address-section glass-card">
      <view class="section-title">收货信息</view>
      <view class="address-content">
        <view class="address-name">{{ order.address.name }} {{ order.address.phone }}</view>
        <view class="address-detail">{{ order.address.province }}{{ order.address.city }}{{ order.address.district }}{{ order.address.detail }}</view>
      </view>
    </view>

    <!-- 商品信息 -->
    <view class="product-section glass-card">
      <view class="section-title">商品信息</view>
      <view class="product-list">
        <view 
          v-for="(item, index) in order.items" 
          :key="index"
          class="product-item"
        >
          <view class="product-image">
            <image :src="item.image" mode="aspectFill" />
          </view>
          <view class="product-info">
            <view class="product-name">{{ item.name }}</view>
            <view class="product-spec">{{ item.size }} / {{ item.material }}</view>
            <view class="product-price-wrap">
              <text class="product-price">¥{{ item.price }}</text>
              <text class="product-quantity">×{{ item.quantity }}</text>
            </view>
          </view>
        </view>
      </view>
      <view class="product-total">
        <text class="total-label">共{{ order.totalItems }}件商品</text>
        <text class="total-value">合计：¥{{ order.totalPrice }}</text>
      </view>
    </view>

    <!-- 底部操作栏 -->
    <view class="bottom-bar glass-card">
      <view class="bottom-actions">
        <button 
          v-if="order.status === 'pending'" 
          class="btn-pay" 
          type="default" 
          @tap="onPay"
        >立即支付</button>
        <button 
          v-if="order.status === 'pending'" 
          class="btn-cancel" 
          type="default" 
          @tap="onCancel"
        >取消订单</button>
        <button 
          v-if="order.status === 'delivered'" 
          class="btn-confirm" 
          type="default" 
          @tap="onConfirm"
        >确认收货</button>
        <button 
          v-if="order.status === 'completed'" 
          class="btn-review" 
          type="default" 
          @tap="onReview"
        >评价</button>
        <button 
          class="btn-contact" 
          type="default" 
          @tap="onContact"
        >联系客服</button>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      order: {
        id: '202401010001',
        status: 'pending',
        statusIcon: '待付款',
        statusText: '待付款',
        statusDesc: '请在24小时内完成付款，超时订单将自动取消',
        statusClass: 'status-pending',
        createTime: '2024-01-01 12:00:00',
        payTime: '',
        shipTime: '',
        receiveTime: '',
        totalItems: 2,
        totalPrice: '48.8',
        address: {
          name: '张三',
          phone: '138****8888',
          province: '广东省',
          city: '深圳市',
          district: '南山区',
          detail: '科技园南区高新南一道9号创维大厦A座'
        },
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
      }
    }
  },
  onLoad(options) {
    if (options.id) {
      // 根据订单ID加载订单详情
      this.loadOrderDetail(options.id)
    }
  },
  methods: {
    loadOrderDetail(id) {
      // 模拟加载订单详情
      console.log('加载订单详情:', id)
    },
    goBack() {
      uni.navigateBack()
    },
    onPay() {
      uni.navigateTo({ 
        url: '/pages/orders/payment?id=' + this.order.id + '&price=' + this.order.totalPrice 
      })
    },
    onCancel() {
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
    onConfirm() {
      uni.showToast({ title: '已确认收货', icon: 'success' })
    },
    onReview() {
      uni.navigateTo({ url: '/pages/orders/review?id=' + this.order.id })
    },
    onContact() {
      uni.showToast({ title: '客服功能开发中', icon: 'none' })
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
  background: rgba(255, 255, 255, 0.78);
  border-bottom: 1rpx solid rgba(255, 255, 255, 0.5);
  backdrop-filter: blur(12px);
  box-shadow: 0 2rpx 12rpx rgba(0, 0, 0, 0.04), 0 4rpx 20rpx rgba(0, 0, 0, 0.03);
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

/* 统一玻璃卡片样式 */
.glass-card {
  position: relative;
  overflow: hidden;
  border-radius: 20rpx;
  background: rgba(255, 255, 255, 0.95);
  box-shadow: 0 2rpx 12rpx rgba(0, 0, 0, 0.08);
}

/* 订单状态 */
.status-section {
  margin: 24rpx 36rpx;
  padding: 40rpx 36rpx;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 16rpx;
  background: rgba(255, 255, 255, 0.95);
  border-radius: 20rpx;
  border: 1rpx solid rgba(255, 255, 255, 0.5);
}

.status-icon {
  width: 80rpx;
  height: 80rpx;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 28rpx;
  font-weight: 600;
  color: #ffffff;
}

.status-pending {
  background: linear-gradient(135deg, #ff6b6b, #ee5a52);
}

.status-shipping {
  background: linear-gradient(135deg, #f5a623, #e6951a);
}

.status-delivered {
  background: linear-gradient(135deg, #66bb6a, #43a047);
}

.status-completed {
  background: linear-gradient(135deg, #666666, #555555);
}

.status-text {
  font-size: 36rpx;
  font-weight: 600;
  color: #333333;
}

.status-desc {
  font-size: 26rpx;
  color: #999999;
  text-align: center;
}

/* 信息区 */
.info-section,
.address-section,
.product-section {
  margin: 0 36rpx 24rpx;
  padding: 32rpx 36rpx;
  background: rgba(255, 255, 255, 0.95);
  border-radius: 20rpx;
  border: 1rpx solid rgba(255, 255, 255, 0.5);
}

.section-title {
  font-size: 32rpx;
  font-weight: 600;
  color: #333333;
  margin-bottom: 24rpx;
}

.info-item {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  padding: 16rpx 0;
  border-bottom: 1rpx solid rgba(0, 0, 0, 0.06);
}

.info-item:last-child {
  border-bottom: none;
}

.info-label {
  font-size: 28rpx;
  color: #666666;
}

.info-value {
  font-size: 28rpx;
  color: #333333;
}

/* 收货信息 */
.address-content {
  display: flex;
  flex-direction: column;
  gap: 12rpx;
}

.address-name {
  font-size: 30rpx;
  font-weight: 600;
  color: #333333;
}

.address-detail {
  font-size: 26rpx;
  color: #666666;
  line-height: 1.5;
}

/* 商品信息 */
.product-list {
  display: flex;
  flex-direction: column;
  gap: 24rpx;
  margin-bottom: 24rpx;
}

.product-item {
  display: flex;
  flex-direction: row;
  align-items: center;
  gap: 20rpx;
  padding-bottom: 24rpx;
  border-bottom: 1rpx solid rgba(0, 0, 0, 0.06);
}

.product-item:last-child {
  border-bottom: none;
  padding-bottom: 0;
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
  min-width: 0;
  display: flex;
  flex-direction: column;
  gap: 8rpx;
}

.product-name {
  font-size: 30rpx;
  font-weight: 600;
  color: #333333;
}

.product-spec {
  font-size: 24rpx;
  color: #999999;
}

.product-price-wrap {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  margin-top: 8rpx;
}

.product-price {
  font-size: 30rpx;
  font-weight: 600;
  color: #66bb6a;
}

.product-quantity {
  font-size: 26rpx;
  color: #999999;
}

.product-total {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  padding-top: 24rpx;
  border-top: 1rpx solid rgba(0, 0, 0, 0.06);
}

.total-label {
  font-size: 28rpx;
  color: #666666;
}

.total-value {
  font-size: 32rpx;
  font-weight: 600;
  color: #66bb6a;
}

/* 底部操作栏 */
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

.bottom-actions {
  display: flex;
  flex-direction: row;
  justify-content: flex-end;
  gap: 16rpx;
}

.bottom-actions button {
  margin: 0;
  padding: 0 32rpx;
  height: 72rpx;
  line-height: 72rpx;
  font-size: 28rpx;
  border-radius: 36rpx;
  border: none;
}

.bottom-actions button::after {
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

.btn-contact {
  background: rgba(0, 0, 0, 0.06);
  color: #666666;
}
</style>