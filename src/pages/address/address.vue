<template>
  <!-- 收货地址页面：延续小程序设计风格 -->
  <view class="page">
    <!-- 状态栏占位 -->
    <view class="status-bar-placeholder"></view>
    
    <!-- 顶部导航栏 -->
    <view class="header">
      <view class="header-left" @tap="goBack">
        <text class="header-icon">←</text>
      </view>
      <text class="header-title">收货地址</text>
      <view class="header-right"></view>
    </view>

    <!-- 地址列表 -->
    <view class="content-wrap">
      <view 
        v-for="(address, index) in addresses" 
        :key="address.id"
        class="address-item"
        :class="{ 'address-default': address.isDefault }"
      >
        <view class="address-info">
          <view class="address-header">
            <text class="address-name">{{ address.name }}</text>
            <text class="address-phone">{{ address.phone }}</text>
            <view v-if="address.isDefault" class="default-tag">默认</view>
          </view>
          <view class="address-detail">
            {{ address.province }}{{ address.city }}{{ address.district }}{{ address.detail }}
          </view>
        </view>
        <view class="address-actions">
          <view class="action-btn" @tap="editAddress(address)">
            <text class="action-icon">编辑</text>
          </view>
          <view class="action-btn" @tap="deleteAddress(address.id)">
            <text class="action-icon">删除</text>
          </view>
        </view>
      </view>

      <!-- 空地址提示 -->
      <view v-if="addresses.length === 0" class="empty-address">
        <text class="empty-icon">地址</text>
        <text class="empty-text">暂无收货地址</text>
        <text class="empty-hint">添加地址后，购物更方便</text>
      </view>
    </view>

    <!-- 底部添加按钮 -->
    <view class="bottom-bar">
      <button class="btn-add" type="default" @tap="addAddress">+ 添加新地址</button>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
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
      ]
    }
  },
  methods: {
    goBack() {
      uni.navigateBack()
    },
    addAddress() {
      uni.showToast({ title: '添加地址功能开发中', icon: 'none' })
    },
    editAddress(address) {
      uni.showToast({ title: `编辑${address.name}的地址`, icon: 'none' })
    },
    deleteAddress(id) {
      uni.showModal({
        title: '提示',
        content: '确定要删除该地址吗？',
        success: (res) => {
          if (res.confirm) {
            this.addresses = this.addresses.filter(addr => addr.id !== id)
            uni.showToast({ title: '删除成功', icon: 'success' })
          }
        }
      })
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

/* 地址列表 */
.content-wrap {
  padding: 24rpx 36rpx;
}

.address-item {
  margin-bottom: 24rpx;
  padding: 32rpx;
  background: rgba(255, 255, 255, 0.95);
  border-radius: 20rpx;
  box-shadow: 0 2rpx 12rpx rgba(0, 0, 0, 0.08);
}

.address-default {
  border: 2rpx solid #66bb6a;
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

.address-actions {
  display: flex;
  flex-direction: row;
  justify-content: flex-end;
  gap: 32rpx;
  margin-top: 24rpx;
  padding-top: 24rpx;
  border-top: 1rpx solid rgba(0, 0, 0, 0.06);
}

.action-btn {
  padding: 12rpx 24rpx;
  background: rgba(0, 0, 0, 0.05);
  border-radius: 8rpx;
}

.action-icon {
  font-size: 26rpx;
  color: #666666;
}

/* 空地址提示 */
.empty-address {
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
}

/* 底部添加按钮 */
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

.btn-add {
  width: 100%;
  height: 80rpx;
  line-height: 80rpx;
  font-size: 32rpx;
  color: #ffffff;
  background: linear-gradient(90deg, #66bb6a, #43a047);
  border-radius: 40rpx;
  border: none;
}

.btn-add::after {
  border: none;
}
</style>