<template>
  <!-- 购物车页面：延续登录页风格，管理已选商品 -->
  <view class="page">
    <!-- 状态栏占位 -->
    <view class="status-bar"></view>

    <!-- 顶部导航栏 -->
    <view class="header glass-card">
      <view class="header-left header-content" @tap="goBack">
        <text class="header-icon">←</text>
      </view>
      <text class="header-title header-content">购物车</text>
      <view class="header-right header-content">
        <text class="header-icon" @tap="onEdit">编辑</text>
      </view>
    </view>

    <!-- 购物车商品列表 -->
    <view class="content-wrap">
      <!-- 商品卡片 -->
      <view
        v-for="(item, index) in cartItems"
        :key="item.id"
        class="glass-card cart-item animate-fade-in"
        :style="{ animationDelay: (index * 0.1) + 's' }"
      >
        <view class="cart-item-left glass-card-content">
          <view class="cart-item-checkbox" :class="{ active: item.selected }" @tap="toggleSelect(index)">
            <text v-if="item.selected" class="checkbox-icon">✓</text>
          </view>
          <view class="cart-item-image">
            <image :src="item.image" mode="aspectFill" />
          </view>
        </view>
        <view class="cart-item-right glass-card-content">
          <view class="cart-item-name">{{ item.name }}</view>
          <view class="cart-item-options">
            <text class="option-text">尺寸：{{ item.size }}</text>
            <text class="option-text">材质：{{ item.material }}</text>
          </view>
          <view class="cart-item-bottom">
            <view class="cart-item-price">¥{{ item.price }}</view>
            <view class="cart-item-quantity">
              <text class="quantity-btn" @tap="decreaseQuantity(index)">-</text>
              <text class="quantity-input">{{ item.quantity }}</text>
              <text class="quantity-btn" @tap="increaseQuantity(index)">+</text>
            </view>
          </view>
        </view>
      </view>

      <!-- 空购物车提示 -->
      <view v-if="cartItems.length === 0" class="empty-cart glass-card">
        <text class="empty-icon glass-card-content">购物车</text>
        <text class="empty-text glass-card-content">购物车是空的</text>
        <text class="empty-hint glass-card-content">快去挑选心仪的可溶性商标吧</text>
        <button class="btn-go-shop glass-card-content" type="default" @tap="goShop">去购物</button>
      </view>
    </view>

    <!-- 底部结算栏 -->
    <view v-if="cartItems.length > 0" class="bottom-bar glass-card">
      <view class="bottom-left bottom-bar-content">
        <view class="cart-item-checkbox" :class="{ active: allSelected }" @tap="toggleAllSelect">
          <text v-if="allSelected" class="checkbox-icon">✓</text>
        </view>
        <text class="select-all-text">全选</text>
      </view>
      <view class="bottom-right bottom-bar-content">
        <view class="total-price-wrap">
          <text class="total-price-label">合计：</text>
          <text class="total-price">¥{{ totalPrice }}</text>
        </view>
        <button class="btn-checkout" type="default" @tap="onCheckout" :disabled="!hasSelectedItems">
          结算({{ selectedCount }})
        </button>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      cartItems: [
        {
          id: '1',
          name: '苹果可溶性商标',
          price: '19.9',
          quantity: 1,
          size: '中尺寸',
          material: '标准材质',
          image: '/static/apple.jpg',
          selected: true
        },
        {
          id: '3',
          name: '草莓可溶性商标',
          price: '28.9',
          quantity: 2,
          size: '中尺寸',
          material: '标准材质',
          image: '/static/草莓.jpg',
          selected: true
        }
      ],
      isEditing: false
    }
  },
  computed: {
    totalPrice() {
      return this.cartItems
        .filter(item => item.selected)
        .reduce((total, item) => total + item.quantity * parseFloat(item.price), 0)
        .toFixed(2)
    },
    selectedCount() {
      return this.cartItems.filter(item => item.selected).length
    },
    hasSelectedItems() {
      return this.selectedCount > 0
    },
    allSelected() {
      return this.cartItems.length > 0 && this.cartItems.every(item => item.selected)
    }
  },
  methods: {
    goBack() {
      uni.navigateBack()
    },
    onEdit() {
      this.isEditing = !this.isEditing
      uni.showToast({ title: this.isEditing ? '编辑模式' : '完成编辑', icon: 'none' })
    },
    toggleSelect(index) {
      this.cartItems[index].selected = !this.cartItems[index].selected
    },
    toggleAllSelect() {
      const newStatus = !this.allSelected
      this.cartItems.forEach(item => {
        item.selected = newStatus
      })
    },
    decreaseQuantity(index) {
      if (this.cartItems[index].quantity > 1) {
        this.cartItems[index].quantity--
      }
    },
    increaseQuantity(index) {
      this.cartItems[index].quantity++
    },
    onCheckout() {
      if (!this.hasSelectedItems) {
        uni.showToast({ title: '请选择商品', icon: 'none' })
        return
      }
      // 跳转到支付页面，传递总价
      uni.navigateTo({ url: '/pages/orders/payment?price=' + this.totalPrice })
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
  font-size: 32rpx;
  color: #666666;
}
.header-title {
  font-size: 32rpx;
  font-weight: 600;
  color: #666666;
}

/* 状态栏占位 */
.status-bar {
  height: var(--status-bar-height);
  width: 100%;
}

/* 内容区 */
.content-wrap {
  padding: 24rpx 36rpx;
  display: flex;
  flex-direction: column;
  gap: 24rpx;
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
  box-shadow: 0 2rpx 12rpx rgba(0, 0, 0, 0.04), 0 4rpx 20rpx rgba(0, 0, 0, 0.03);
  border-radius: 20rpx;
  z-index: 0;
}

.glass-card-content {
  position: relative;
  z-index: 1;
}

/* 购物车商品项 */
.cart-item {
  padding: 24rpx;
  display: flex;
  flex-direction: row;
  align-items: flex-start;
  gap: 20rpx;
}

.cart-item-left {
  display: flex;
  align-items: flex-start;
  gap: 16rpx;
}

/* 复选框 */
.cart-item-checkbox {
  width: 40rpx;
  height: 40rpx;
  border-radius: 50%;
  border: 2rpx solid #c4cae0;
  background: #ffffff;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top: 8rpx;
  transition: all 0.2s ease;
}

.cart-item-checkbox.active {
  background: #66bb6a;
  border-color: #66bb6a;
}

.checkbox-icon {
  font-size: 24rpx;
  color: #ffffff;
  font-weight: bold;
}

/* 商品图片 */
.cart-item-image {
  width: 160rpx;
  height: 160rpx;
  border-radius: 12rpx;
  overflow: hidden;
  background: rgba(255, 255, 255, 0.5);
}

.cart-item-image image {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

/* 商品信息 */
.cart-item-right {
  flex: 1;
  min-width: 0;
  display: flex;
  flex-direction: column;
  gap: 12rpx;
}

.cart-item-name {
  font-size: 28rpx;
  font-weight: 600;
  color: #666666;
  line-height: 1.4;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.cart-item-options {
  display: flex;
  flex-wrap: wrap;
  gap: 12rpx;
}

.option-text {
  font-size: 22rpx;
  color: #999999;
  background: rgba(0, 0, 0, 0.05);
  padding: 4rpx 12rpx;
  border-radius: 8rpx;
}

.cart-item-bottom {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  margin-top: 8rpx;
}

.cart-item-price {
  font-size: 32rpx;
  font-weight: 600;
  color: #66bb6a;
}

/* 数量选择器 */
.cart-item-quantity {
  display: flex;
  align-items: center;
  gap: 16rpx;
}

.quantity-btn {
  width: 48rpx;
  height: 48rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 28rpx;
  color: #666666;
  background: rgba(0, 0, 0, 0.06);
  border-radius: 8rpx;
  transition: all 0.2s ease;
}

.quantity-btn:active {
  transform: scale(0.95);
  background: rgba(0, 0, 0, 0.1);
}

.quantity-input {
  font-size: 28rpx;
  color: #666666;
  min-width: 40rpx;
  text-align: center;
}

/* 空购物车 */
.empty-cart {
  padding: 80rpx 36rpx;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 24rpx;
  min-height: 400rpx;
}

.empty-icon {
  font-size: 80rpx;
}

.empty-text {
  font-size: 32rpx;
  font-weight: 600;
  color: #666666;
}

.empty-hint {
  font-size: 24rpx;
  color: #999999;
}

.btn-go-shop {
  margin-top: 20rpx;
  padding: 0 48rpx;
  height: 72rpx;
  line-height: 72rpx;
  font-size: 28rpx;
  color: #ffffff;
  background: linear-gradient(90deg, #66bb6a, #43a047);
  border-radius: 36rpx;
  border: none;
}

.btn-go-shop::after {
  border: none;
}

/* 底部结算栏 */
.bottom-bar {
  position: fixed;
  left: 0;
  right: 0;
  bottom: 0;
  padding: 20rpx 36rpx;
  padding-bottom: calc(20rpx + env(safe-area-inset-bottom));
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  position: relative;
  overflow: hidden;
}

.bottom-bar::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.8), rgba(255, 255, 255, 0.2));
  border-top: 1rpx solid rgba(255, 255, 255, 0.5);
  backdrop-filter: blur(14px);
  box-shadow: 0 -2rpx 12rpx rgba(0, 0, 0, 0.03);
  z-index: 0;
}

.bottom-bar-content {
  position: relative;
  z-index: 1;
}

.bottom-left {
  display: flex;
  align-items: center;
  gap: 12rpx;
}

.select-all-text {
  font-size: 28rpx;
  color: #666666;
}

.bottom-right {
  display: flex;
  align-items: center;
  gap: 24rpx;
}

.total-price-wrap {
  display: flex;
  align-items: baseline;
  gap: 8rpx;
}

.total-price-label {
  font-size: 24rpx;
  color: #666666;
}

.total-price {
  font-size: 36rpx;
  font-weight: 600;
  color: #66bb6a;
}

.btn-checkout {
  padding: 0 48rpx;
  height: 80rpx;
  line-height: 80rpx;
  font-size: 28rpx;
  color: #ffffff;
  background: linear-gradient(90deg, #66bb6a, #43a047);
  border-radius: 40rpx;
  border: none;
  transition: all 0.2s ease;
}

.btn-checkout:active {
  transform: scale(0.98);
  opacity: 0.9;
}

.btn-checkout::after {
  border: none;
}

.btn-checkout:disabled {
  background: rgba(102, 187, 106, 0.5);
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