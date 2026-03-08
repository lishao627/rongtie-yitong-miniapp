<template>
  <!-- 注册页：全屏渐变背景 + 玻璃卡片（与登录页同风格） -->
  <view class="page">
    <view class="screen theme-purple">
      <view class="glass-card">
        <!-- 标题区 -->
        <view class="welcome">创建账号</view>
        <view class="welcome-sub">仅需账号与密码即可完成注册</view>

        <!-- 注册表单（仅账号密码） -->
        <view class="form">
          <!-- 用户名 -->
          <view>
            <view class="field-label">用户名</view>
            <view class="field">
              <input
                v-model="form.username"
                class="input"
                placeholder="请输入用户名"
                confirm-type="next"
              />
              <view class="input-icon-box">
                <text class="input-icon">👤</text>
              </view>
            </view>
          </view>

          <!-- 密码 -->
          <view>
            <view class="field-label">密码</view>
            <view class="field">
              <input
                v-model="form.password"
                class="input"
                type="password"
                placeholder="请输入密码"
                confirm-type="done"
              />
              <view class="input-icon-box">
                <text class="input-icon">🔒</text>
              </view>
            </view>
          </view>

          <!-- 注册按钮 -->
          <button class="btn" type="default" @tap="onRegister" :disabled="!canSubmit">
            注册
          </button>

          <!-- 底部提示 -->
          <view class="login-tip">
            已有账号？
            <text class="login-link" @tap="goLogin">去登录</text>
          </view>
        </view>
      </view>

      <!-- Toast 提示 -->
      <view class="toast" :class="{ show: toast.show }">
        <view class="toast-dot" />
        <text class="toast-text">{{ toast.text }}</text>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      form: {
        username: '',
        password: '',
      },
      toast: {
        show: false,
        text: '',
      },
      toastTimer: null,
    }
  },
  computed: {
    canSubmit() {
      return this.form.username.trim() && this.form.password.trim()
    },
  },
  methods: {
    showToast(text) {
      this.toast.text = text
      this.toast.show = true
      if (this.toastTimer) clearTimeout(this.toastTimer)
      this.toastTimer = setTimeout(() => {
        this.toast.show = false
      }, 2000)
    },
    onRegister() {
      if (!this.canSubmit) {
        this.showToast('请输入完整的账号和密码')
        return
      }
      // 原型交互：模拟注册成功，然后返回登录
      this.showToast('注册成功：已返回登录页')
      setTimeout(() => {
        uni.redirectTo({ url: '/pages/login/login' })
      }, 600)
    },
    goLogin() {
      uni.redirectTo({ url: '/pages/login/login' })
    },
  },
}
</script>

<style>
/* 页面整体：全屏渐变背景 + 居中卡片（与登录页一致） */
.page {
  flex: 1;
  flex-direction: column;
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  width: 100%;
  padding: 40rpx;
  background-image: radial-gradient(circle at 10% 0, #e8f5e9 0, #c8e6c9 40%, #a5d6a7 70%, #81c784 100%);
  background-repeat: no-repeat;
  background-size: cover;
  box-sizing: border-box;
}

.screen {
  width: 100%;
  max-width: 900rpx;
  border-radius: 32rpx;
  padding: 120rpx 40rpx 80rpx;
  box-sizing: border-box;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
}

.theme-purple {
  /* 占位，保留 class 语义 */
}

/* 玻璃卡片 */
.glass-card {
  width: 100%;
  background-image: linear-gradient(180deg, rgba(255, 255, 255, 0.78) 0%, rgba(255, 255, 255, 0.62) 100%);
  border-width: 1rpx;
  border-style: solid;
  border-color: rgba(255, 255, 255, 0.55);
  backdrop-filter: blur(14px);
  border-radius: 24rpx;
  padding: 40rpx 36rpx 32rpx;
  box-shadow: 0 24rpx 48rpx rgba(0, 0, 0, 0.08);
}

/* 标题 */
.welcome {
  text-align: center;
  font-size: 34rpx;
  font-weight: 600;
  color: #333333;
}

.welcome-sub {
  margin-top: 12rpx;
  text-align: center;
  font-size: 24rpx;
  line-height: 1.4;
  color: #666666;
}

/* 表单 */
.form {
  margin-top: 40rpx;
  width: 100%;
  font-size: 28rpx;
  display: flex;
  flex-direction: column;
  row-gap: 40rpx;
}

.field-label {
  font-size: 22rpx;
  color: #7e8496;
  margin-bottom: 10rpx;
}

.field {
  position: relative;
  display: flex;
  flex-direction: column;
  width: 100%;
}

/* 输入框 */
.input {
  width: 100%;
  height: 88rpx;
  box-sizing: border-box;
  border-radius: 16rpx;
  padding: 0 80rpx 0 32rpx;
  font-size: 28rpx;
  background-color: #ffffff;
  border-width: 1rpx;
  border-style: solid;
  border-color: #e0e3e8;
}

.input:focus {
  border-color: #66bb6a;
}

/* 右侧小图标 */
.input-icon-box {
  position: absolute;
  right: 20rpx;
  top: 50%;
  margin-top: -24rpx;
  width: 48rpx;
  height: 48rpx;
  border-radius: 24rpx;
  background-image: linear-gradient(135deg, #e0e7ff, #fef9c3);
  align-items: center;
  justify-content: center;
}

.input-icon {
  font-size: 26rpx;
  font-weight: 900;
  color: #4b5563;
}

/* 注册按钮 */
.btn {
  margin-top: 60rpx;
  width: 100%;
  height: 88rpx;
  border-radius: 44rpx;
  font-size: 32rpx;
  font-weight: 500;
  color: #ffffff;
  background-image: linear-gradient(90deg, #66bb6a, #43a047);
  box-shadow: 0 20rpx 40rpx rgba(102, 187, 106, 0.6);
  align-items: center;
  justify-content: center;
}

.btn:active {
  transform: scale(0.97);
  box-shadow: 0 12rpx 26rpx rgba(102, 187, 106, 0.5);
}

.btn[disabled] {
  background-image: linear-gradient(90deg, #a5d6a7, #81c784);
  box-shadow: 0 20rpx 40rpx rgba(129, 199, 132, 0.4);
}

/* 底部提示 */
.login-tip {
  margin-top: 40rpx;
  text-align: center;
  font-size: 24rpx;
  color: #999999;
}

.login-link {
  color: #66bb6a;
}

/* Toast */
.toast {
  position: absolute;
  bottom: 40rpx;
  left: 50%;
  transform: translateX(-50%);
  padding: 12rpx 24rpx;
  border-radius: 999rpx;
  background-color: rgba(26, 32, 54, 0.96);
  flex-direction: row;
  display: none;
  align-items: center;
}

.toast.show {
  display: flex;
}

.toast-dot {
  width: 14rpx;
  height: 14rpx;
  border-radius: 50%;
  background-color: #7fffd4;
  margin-right: 12rpx;
}

.toast-text {
  font-size: 22rpx;
  color: #ffffff;
}
</style>

