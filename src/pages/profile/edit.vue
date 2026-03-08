<template>
  <!-- 编辑个人资料页面：延续小程序设计风格 -->
  <view class="page">
    <!-- 顶部导航栏 -->
    <view class="header">
      <view class="header-left" @tap="goBack">
        <text class="header-icon">←</text>
      </view>
      <text class="header-title">编辑资料</text>
      <view class="header-right" @tap="saveProfile">
        <text class="header-action">保存</text>
      </view>
    </view>

    <!-- 头像编辑 -->
    <view class="avatar-section">
      <view class="avatar-wrap" @tap="changeAvatar">
        <image v-if="form.avatar" :src="form.avatar" mode="aspectFill" class="avatar-image" />
        <view v-else class="avatar-placeholder">{{ form.name ? form.name.charAt(0) : '用' }}</view>
        <view class="avatar-mask">
          <text class="avatar-text">更换头像</text>
        </view>
      </view>
    </view>

    <!-- 表单 -->
    <view class="form-section">
      <view class="form-item">
        <text class="form-label">用户名</text>
        <input 
          class="form-input" 
          v-model="form.name" 
          placeholder="请输入用户名"
          maxlength="20"
        />
      </view>
      <view class="form-item">
        <text class="form-label">手机号</text>
        <input 
          class="form-input" 
          v-model="form.phone" 
          placeholder="请输入手机号"
          type="number"
          maxlength="11"
        />
      </view>
      <view class="form-item">
        <text class="form-label">微信号</text>
        <input 
          class="form-input" 
          v-model="form.wechat" 
          placeholder="请输入微信号"
          maxlength="30"
        />
      </view>
      <view class="form-item">
        <text class="form-label">收货地址</text>
        <textarea 
          class="form-textarea" 
          v-model="form.address" 
          placeholder="请输入收货地址"
          maxlength="100"
        />
      </view>
    </view>

    <!-- 提示 -->
    <view class="tips">
      <text class="tips-text">完善个人信息，享受更好的服务体验</text>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      form: {
        name: '溶贴易通用户',
        avatar: '',
        phone: '138****8888',
        wechat: 'wx_123456',
        address: '广东省深圳市南山区科技园南区'
      }
    }
  },
  onLoad() {
    // 加载用户信息
    const userInfo = uni.getStorageSync('userInfo')
    if (userInfo) {
      this.form = { ...this.form, ...userInfo }
    }
  },
  methods: {
    goBack() {
      uni.navigateBack()
    },
    changeAvatar() {
      uni.chooseImage({
        count: 1,
        sizeType: ['compressed'],
        sourceType: ['album', 'camera'],
        success: (res) => {
          this.form.avatar = res.tempFilePaths[0]
          uni.showToast({ title: '头像已选择', icon: 'success' })
        }
      })
    },
    saveProfile() {
      // 验证表单
      if (!this.form.name.trim()) {
        uni.showToast({ title: '请输入用户名', icon: 'none' })
        return
      }
      if (!this.form.phone.trim()) {
        uni.showToast({ title: '请输入手机号', icon: 'none' })
        return
      }
      
      // 保存到本地存储
      uni.setStorageSync('userInfo', this.form)
      uni.setStorageSync('updatedUserInfo', this.form)
      
      uni.showToast({ 
        title: '保存成功', 
        icon: 'success',
        success: () => {
          setTimeout(() => {
            uni.navigateBack()
          }, 1500)
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
  font-weight: 600;
}

/* 头像编辑 */
.avatar-section {
  display: flex;
  justify-content: center;
  padding: 48rpx 36rpx;
}

.avatar-wrap {
  width: 200rpx;
  height: 200rpx;
  border-radius: 50%;
  overflow: hidden;
  position: relative;
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
  font-size: 80rpx;
  color: #ffffff;
}

.avatar-mask {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 60rpx;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
}

.avatar-text {
  font-size: 24rpx;
  color: #ffffff;
}

/* 表单 */
.form-section {
  margin: 0 36rpx;
  padding: 32rpx 36rpx;
  background: rgba(255, 255, 255, 0.95);
  border-radius: 20rpx;
  box-shadow: 0 2rpx 12rpx rgba(0, 0, 0, 0.08);
}

.form-item {
  display: flex;
  flex-direction: column;
  gap: 16rpx;
  padding: 28rpx 0;
  border-bottom: 1rpx solid rgba(0, 0, 0, 0.06);
}

.form-item:last-child {
  border-bottom: none;
}

.form-label {
  font-size: 28rpx;
  color: #666666;
}

.form-input {
  font-size: 32rpx;
  color: #333333;
  height: 60rpx;
  line-height: 60rpx;
}

.form-textarea {
  font-size: 32rpx;
  color: #333333;
  height: 120rpx;
  line-height: 1.5;
}

/* 提示 */
.tips {
  display: flex;
  justify-content: center;
  padding: 48rpx 36rpx;
}

.tips-text {
  font-size: 26rpx;
  color: #999999;
}
</style>