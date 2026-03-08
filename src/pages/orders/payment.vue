<template>
  <!-- 支付页面：延续小程序设计风格 -->
  <view class="page">
    <!-- 顶部导航栏 -->
    <view class="header">
      <view class="header-left" @tap="goBack">
        <text class="header-icon">←</text>
      </view>
      <text class="header-title">确认支付</text>
      <view class="header-right"></view>
    </view>

    <!-- 顶部状态栏占位 -->
    <view class="status-bar-placeholder"></view>

    <!-- 收货地址选择 -->
    <view class="address-section" @tap="selectAddress">
      <view v-if="selectedAddress" class="address-content">
        <view class="address-header">
          <text class="address-name">{{ selectedAddress.name }}</text>
          <text class="address-phone">{{ selectedAddress.phone }}</text>
          <view v-if="selectedAddress.isDefault" class="default-tag">默认</view>
        </view>
        <view class="address-detail">
          {{ selectedAddress.province }}{{ selectedAddress.city }}{{ selectedAddress.district }}{{ selectedAddress.detail }}
        </view>
      </view>
      <view v-else class="address-empty">
        <text class="address-icon">📍</text>
        <text class="address-text">请选择收货地址</text>
      </view>
      <view class="address-arrow">→</view>
    </view>

    <!-- 支付金额区域 -->
    <view class="amount-section">
      <text class="amount-label">支付金额</text>
      <view class="amount-value">
        <text class="amount-symbol">¥</text>
        <text class="amount-number">{{ order.totalPrice }}</text>
      </view>
      <text class="amount-desc">订单号：{{ order.id }}</text>
    </view>

    <!-- 支付方式选择 -->
    <view class="payment-section">
      <view class="section-title">选择支付方式</view>
      <view class="payment-list">
        <view 
          v-for="(method, index) in paymentMethods" 
          :key="index"
          class="payment-item"
          :class="{ active: selectedMethod === method.value }"
          @tap="selectMethod(method.value)"
        >
          <view class="payment-icon">{{ method.icon }}</view>
          <view class="payment-info">
            <text class="payment-name">{{ method.name }}</text>
            <text class="payment-desc">{{ method.desc }}</text>
          </view>
          <view class="payment-check">
            <view v-if="selectedMethod === method.value" class="check-circle">✓</view>
            <view v-else class="uncheck-circle"></view>
          </view>
        </view>
      </view>
    </view>

    <!-- 订单信息 -->
    <view class="order-info-section">
      <view class="section-title">订单信息</view>
      <view class="info-item">
        <text class="info-label">商品总额</text>
        <text class="info-value">¥{{ order.totalPrice }}</text>
      </view>
      <view class="info-item">
        <text class="info-label">运费</text>
        <text class="info-value free">免运费</text>
      </view>
      <view class="info-item total">
        <text class="info-label">实付金额</text>
        <text class="info-value total-price">¥{{ order.totalPrice }}</text>
      </view>
    </view>

    <!-- 底部支付按钮 -->
    <view class="bottom-bar">
      <view class="bottom-content">
        <view class="bottom-amount">
          <text class="bottom-label">实付金额：</text>
          <text class="bottom-price">¥{{ order.totalPrice }}</text>
        </view>
        <button class="btn-pay" type="default" @tap="onPay">立即支付</button>
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
        totalPrice: '48.8'
      },
      selectedMethod: 'wechat',
      selectedAddress: null,
      addresses: [
        {
          id: '1',
          name: '张三',
          phone: '138****8888',
          province: '广东省',
          city: '深圳市',
          district: '南山区',
          detail: '科技园南区高新南一道9号创维大厦A座',
          isDefault: true
        },
        {
          id: '2',
          name: '李四',
          phone: '139****6666',
          province: '广东省',
          city: '广州市',
          district: '天河区',
          detail: '珠江新城华夏路10号',
          isDefault: false
        }
      ],
      paymentMethods: [
        {
          value: 'wechat',
          name: '微信支付',
          desc: '推荐使用微信支付',
          icon: '微信'
        },
        {
          value: 'alipay',
          name: '支付宝',
          desc: '支付宝支付',
          icon: '支付宝'
        },
        {
          value: 'balance',
          name: '余额支付',
          desc: '当前余额：¥0.00',
          icon: '余额'
        }
      ]
    }
  },
  onLoad(options) {
    if (options.id) {
      this.order.id = options.id
    }
    if (options.price) {
      this.order.totalPrice = options.price
    }
    // 默认选中默认地址
    const defaultAddress = this.addresses.find(addr => addr.isDefault)
    if (defaultAddress) {
      this.selectedAddress = defaultAddress
    }
  },
  methods: {
    goBack() {
      uni.navigateBack()
    },
    selectMethod(value) {
      this.selectedMethod = value
    },
    selectAddress() {
      // 显示地址选择弹窗或跳转到地址列表页
      const addressList = this.addresses.map((addr, index) => {
        const isDefault = addr.isDefault ? '【默认】' : ''
        return `${isDefault}${addr.name} ${addr.phone}\n${addr.province}${addr.city}${addr.district}${addr.detail}`
      })
      addressList.push('+ 添加新地址')
      
      uni.showActionSheet({
        itemList: addressList,
        success: (res) => {
          if (res.tapIndex === addressList.length - 1) {
            // 点击了添加新地址
            this.addNewAddress()
          } else {
            // 选择了已有地址
            this.selectedAddress = this.addresses[res.tapIndex]
          }
        }
      })
    },
    addNewAddress() {
      uni.navigateTo({
        url: '/pages/address/address'
      })
    },
    onPay() {
      if (!this.selectedAddress) {
        uni.showToast({ title: '请选择收货地址', icon: 'none' })
        return
      }
      uni.showLoading({ title: '支付中...' })
      setTimeout(() => {
        uni.hideLoading()
        uni.showToast({ title: '支付成功', icon: 'success' })
        setTimeout(() => {
          uni.redirectTo({ url: '/pages/orders/orders' })
        }, 1500)
      }, 2000)
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
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 100;
  height: 100rpx;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  padding: 0 36rpx;
  padding-top: var(--status-bar-height, 0);
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
}

/* 状态栏占位 */
.status-bar-placeholder {
  height: calc(100rpx + var(--status-bar-height, 0));
}

/* 收货地址区域 */
.address-section {
  margin: 24rpx 36rpx;
  padding: 32rpx 36rpx;
  background: rgba(255, 255, 255, 0.95);
  border-radius: 20rpx;
  box-shadow: 0 2rpx 12rpx rgba(0, 0, 0, 0.08);
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
}

.address-content {
  flex: 1;
}

.address-header {
  display: flex;
  flex-direction: row;
  align-items: center;
  gap: 20rpx;
  margin-bottom: 16rpx;
}

.address-name {
  font-size: 32rpx;
  font-weight: 600;
  color: #333333;
}

.address-phone {
  font-size: 28rpx;
  color: #666666;
}

.default-tag {
  padding: 4rpx 16rpx;
  font-size: 22rpx;
  color: #ffffff;
  background: #66bb6a;
  border-radius: 12rpx;
}

.address-detail {
  font-size: 28rpx;
  color: #666666;
  line-height: 1.5;
}

.address-empty {
  flex: 1;
  display: flex;
  flex-direction: row;
  align-items: center;
  gap: 16rpx;
}

.address-icon {
  font-size: 40rpx;
}

.address-text {
  font-size: 30rpx;
  color: #66bb6a;
}

.address-arrow {
  font-size: 32rpx;
  color: #999999;
}

/* 支付金额区域 */
.amount-section {
  margin: 0 36rpx 24rpx;
  padding: 48rpx 36rpx;
  background: rgba(255, 255, 255, 0.95);
  border-radius: 20rpx;
  box-shadow: 0 2rpx 12rpx rgba(0, 0, 0, 0.08);
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 16rpx;
}

.amount-label {
  font-size: 28rpx;
  color: #666666;
}

.amount-value {
  display: flex;
  align-items: baseline;
  gap: 8rpx;
}

.amount-symbol {
  font-size: 40rpx;
  color: #333333;
  font-weight: 600;
}

.amount-number {
  font-size: 72rpx;
  color: #333333;
  font-weight: 700;
}

.amount-desc {
  font-size: 24rpx;
  color: #999999;
}

/* 支付方式选择 */
.payment-section {
  margin: 0 36rpx 24rpx;
  padding: 32rpx 36rpx;
  background: rgba(255, 255, 255, 0.95);
  border-radius: 20rpx;
  box-shadow: 0 2rpx 12rpx rgba(0, 0, 0, 0.08);
}

.section-title {
  font-size: 32rpx;
  font-weight: 600;
  color: #333333;
  margin-bottom: 24rpx;
}

.payment-list {
  display: flex;
  flex-direction: column;
  gap: 20rpx;
}

.payment-item {
  display: flex;
  flex-direction: row;
  align-items: center;
  padding: 24rpx;
  background: rgba(0, 0, 0, 0.03);
  border-radius: 16rpx;
  border: 2rpx solid transparent;
  transition: all 0.3s ease;
}

.payment-item.active {
  background: rgba(102, 187, 106, 0.1);
  border-color: #66bb6a;
}

.payment-icon {
  width: 60rpx;
  height: 60rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 28rpx;
  color: #66bb6a;
  background: rgba(102, 187, 106, 0.2);
  border-radius: 12rpx;
  margin-right: 20rpx;
}

.payment-info {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 8rpx;
}

.payment-name {
  font-size: 30rpx;
  font-weight: 600;
  color: #333333;
}

.payment-desc {
  font-size: 24rpx;
  color: #999999;
}

.payment-check {
  width: 44rpx;
  height: 44rpx;
  display: flex;
  align-items: center;
  justify-content: center;
}

.check-circle {
  width: 44rpx;
  height: 44rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 24rpx;
  color: #ffffff;
  background: #66bb6a;
  border-radius: 50%;
}

.uncheck-circle {
  width: 44rpx;
  height: 44rpx;
  border: 2rpx solid #cccccc;
  border-radius: 50%;
}

/* 订单信息 */
.order-info-section {
  margin: 0 36rpx 24rpx;
  padding: 32rpx 36rpx;
  background: rgba(255, 255, 255, 0.95);
  border-radius: 20rpx;
  box-shadow: 0 2rpx 12rpx rgba(0, 0, 0, 0.08);
}

.info-item {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  padding: 20rpx 0;
  border-bottom: 1rpx solid rgba(0, 0, 0, 0.06);
}

.info-item:last-child {
  border-bottom: none;
}

.info-item.total {
  margin-top: 10rpx;
  padding-top: 30rpx;
  border-top: 2rpx solid rgba(0, 0, 0, 0.1);
}

.info-label {
  font-size: 28rpx;
  color: #666666;
}

.info-value {
  font-size: 28rpx;
  color: #333333;
}

.info-value.free {
  color: #66bb6a;
}

.info-value.total-price {
  font-size: 36rpx;
  color: #66bb6a;
  font-weight: 700;
}

/* 底部支付按钮 */
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

.bottom-amount {
  display: flex;
  align-items: baseline;
  gap: 8rpx;
}

.bottom-label {
  font-size: 28rpx;
  color: #666666;
}

.bottom-price {
  font-size: 40rpx;
  color: #66bb6a;
  font-weight: 700;
}

.btn-pay {
  padding: 0 56rpx;
  height: 80rpx;
  line-height: 80rpx;
  font-size: 32rpx;
  color: #ffffff;
  background: linear-gradient(90deg, #66bb6a, #43a047);
  border-radius: 40rpx;
  border: none;
  margin: 0;
}

.btn-pay::after {
  border: none;
}

.btn-pay:active {
  transform: scale(0.98);
  opacity: 0.9;
}
</style>
