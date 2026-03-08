<template>
  <view class="splash-page">
    <image class="bg-image" src="/static/背景图片.png" mode="aspectFill" />
    <view class="countdown">{{ countdown }}秒后进入首页</view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      countdown: 3,
      timer: null
    }
  },
  onLoad() {
    this.startCountdown()
  },
  onUnload() {
    if (this.timer) {
      clearInterval(this.timer)
    }
  },
  methods: {
    startCountdown() {
      this.timer = setInterval(() => {
        this.countdown--
        if (this.countdown <= 0) {
          clearInterval(this.timer)
          this.goToHome()
        }
      }, 1000)
    },
    goToHome() {
      uni.switchTab({ url: '/pages/index/index' })
    }
  }
}
</script>

<style>
.splash-page {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  width: 100vw;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #000;
}

.bg-image {
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
}

.countdown {
  position: absolute;
  bottom: 100rpx;
  right: 40rpx;
  padding: 16rpx 32rpx;
  background-color: rgba(0, 0, 0, 0.5);
  border-radius: 32rpx;
  font-size: 28rpx;
  color: #ffffff;
  z-index: 10;
}
</style>
