<template>
  <!-- 个人资料页面：延续小程序设计风格 -->
  <view class="page">
    <!-- 状态栏占位 -->
    <view class="status-bar"></view>
    
    <!-- 顶部导航栏 -->
    <view class="header">
      <view class="header-left" @tap="goBack">
        <text class="header-icon">←</text>
      </view>
      <text class="header-title">个人资料</text>
      <view class="header-right" @tap="goEdit">
        <text class="header-action">编辑</text>
      </view>
    </view>

    <!-- 用户信息卡片 -->
    <view class="user-card">
      <view class="avatar-section">
        <view class="avatar-wrap">
          <image v-if="userInfo.avatar" :src="userInfo.avatar" mode="aspectFill" class="avatar-image" />
          <view v-else class="avatar-placeholder">{{ userInfo.name ? userInfo.name.charAt(0) : '用' }}</view>
        </view>
        <text class="user-name">{{ userInfo.name }}</text>
        <text class="user-level">{{ userInfo.level }}</text>
      </view>

      <view class="info-list">
        <view class="info-item">
          <text class="info-label">用户名</text>
          <text class="info-value">{{ userInfo.name }}</text>
        </view>
        <view class="info-item">
          <text class="info-label">手机号</text>
          <text class="info-value">{{ userInfo.phone }}</text>
        </view>
        <view class="info-item">
          <text class="info-label">微信号</text>
          <text class="info-value">{{ userInfo.wechat || '未绑定' }}</text>
        </view>
        <view class="info-item">
          <text class="info-label">收货地址</text>
          <text class="info-value address">{{ userInfo.address || '未设置' }}</text>
        </view>
        <view class="info-item">
          <text class="info-label">积分</text>
          <text class="info-value points">{{ userInfo.points }}</text>
        </view>
      </view>
    </view>

    <!-- 账号安全 -->
    <view class="security-section">
      <view class="section-title">账号安全</view>
      <view class="security-list">
        <view class="security-item" @tap="changePassword">
          <text class="security-label">修改密码</text>
          <text class="security-arrow">></text>
        </view>
        <view class="security-item" @tap="bindPhone">
          <text class="security-label">绑定手机</text>
          <text class="security-status">已绑定</text>
        </view>
        <view class="security-item" @tap="bindWechat">
          <text class="security-label">绑定微信</text>
          <text class="security-status" :class="{ 'status-unbound': !userInfo.wechat }">
            {{ userInfo.wechat ? '已绑定' : '未绑定' }}
          </text>
        </view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      userInfo: {
        name: '溶贴易通用户',
        avatar: '',
        phone: '138****8888',
        wechat: 'wx_123456',
        address: '广东省深圳市南山区科技园南区',
        level: '普通会员',
        points: 128
      }
    }
  },
  onShow() {
    // 检查是否有更新的用户信息
    const updatedInfo = uni.getStorageSync('updatedUserInfo')
    if (updatedInfo) {
      this.userInfo = { ...this.userInfo, ...updatedInfo }
      uni.removeStorageSync('updatedUserInfo')
    }
  },
  methods: {
    goBack() {
      uni.navigateBack()
    },
    goEdit() {
      uni.navigateTo({ 
        url: '/pages/profile/edit'
      })
    },
    changePassword() {
      uni.showToast({ title: '修改密码功能开发中', icon: 'none' })
    },
    bindPhone() {
      uni.showToast({ title: '已绑定手机号', icon: 'none' })
    },
    bindWechat() {
      if (this.userInfo.wechat) {
        uni.showToast({ title: '已绑定微信', icon: 'none' })
      } else {
        uni.showToast({ title: '绑定微信功能开发中', icon: 'none' })
      }
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

/* 状态栏占位 */
.status-bar {
  height: var(--status-bar-height);
  width: 100%;
  background: rgba(255, 255, 255, 0.95);
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

/* 用户信息卡片 */
.user-card {
  margin: 24rpx 36rpx;
  padding: 48rpx 36rpx;
  background: rgba(255, 255, 255, 0.95);
  border-radius: 20rpx;
  box-shadow: 0 2rpx 12rpx rgba(0, 0, 0, 0.08);
}

.avatar-section {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 16rpx;
  margin-bottom: 48rpx;
}

.avatar-wrap {
  width: 160rpx;
  height: 160rpx;
  border-radius: 50%;
  overflow: hidden;
  background: linear-gradient(135deg, #66bb6a, #43a047);
}

.avatar-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.avatar-placeholder {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 64rpx;
  color: #ffffff;
}

.user-name {
  font-size: 40rpx;
  font-weight: 700;
  color: #333333;
}

.user-level {
  font-size: 26rpx;
  color: #66bb6a;
  background: rgba(102, 187, 106, 0.1);
  padding: 8rpx 24rpx;
  border-radius: 20rpx;
}

/* 信息列表 */
.info-list {
  display: flex;
  flex-direction: column;
}

.info-item {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  padding: 28rpx 0;
  border-bottom: 1rpx solid rgba(0, 0, 0, 0.06);
}

.info-item:last-child {
  border-bottom: none;
}

.info-label {
  font-size: 30rpx;
  color: #666666;
}

.info-value {
  font-size: 30rpx;
  color: #333333;
  font-weight: 500;
}

.info-value.address {
  max-width: 400rpx;
  text-align: right;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.info-value.points {
  color: #66bb6a;
  font-weight: 700;
}

/* 账号安全 */
.security-section {
  margin: 0 36rpx;
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

.security-list {
  display: flex;
  flex-direction: column;
}

.security-item {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  padding: 28rpx 0;
  border-bottom: 1rpx solid rgba(0, 0, 0, 0.06);
}

.security-item:last-child {
  border-bottom: none;
}

.security-label {
  font-size: 30rpx;
  color: #333333;
}

.security-arrow {
  font-size: 28rpx;
  color: #999999;
}

.security-status {
  font-size: 26rpx;
  color: #66bb6a;
}

.security-status.status-unbound {
  color: #999999;
}
</style>