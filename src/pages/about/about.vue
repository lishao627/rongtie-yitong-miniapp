<template>
  <!-- 关于我们页面：延续小程序设计风格 -->
  <view class="page">
    <!-- 顶部导航栏 -->
    <view class="header">
      <view class="header-left" @tap="goBack">
        <text class="header-icon">←</text>
      </view>
      <text class="header-title">关于我们</text>
      <view class="header-right"></view>
    </view>

    <!-- 品牌信息 -->
    <view class="brand-section">
      <view class="brand-logo">溶贴易通</view>
      <text class="brand-name">溶贴易通</text>
      <text class="brand-slogan">专业可溶性商标定制平台</text>
      <text class="brand-version">版本号：v1.0.0</text>
    </view>

    <!-- 公司简介 -->
    <view class="intro-section">
      <view class="section-title">公司简介</view>
      <text class="intro-text">
        溶贴易通是一家专注于可溶性商标研发、生产和销售的专业平台。我们致力于为客户提供食品级安全、环保可降解的商标产品，广泛应用于水果、蔬菜等农产品的标识需求。
      </text>
      <text class="intro-text">
        公司拥有先进的生产设备和完善的质量管理体系，所有产品均通过严格的质量检测，确保符合国家相关标准。我们秉承"质量第一、客户至上"的经营理念，为客户提供优质的产品和服务。
      </text>
    </view>

    <!-- 核心优势 -->
    <view class="advantage-section">
      <view class="section-title">核心优势</view>
      <view class="advantage-list">
        <view class="advantage-item">
          <view class="advantage-icon">✓</view>
          <view class="advantage-info">
            <text class="advantage-title">食品级安全</text>
            <text class="advantage-desc">采用食品级材料，安全无害</text>
          </view>
        </view>
        <view class="advantage-item">
          <view class="advantage-icon">✓</view>
          <view class="advantage-info">
            <text class="advantage-title">环保可降解</text>
            <text class="advantage-desc">环保材料，自然降解无污染</text>
          </view>
        </view>
        <view class="advantage-item">
          <view class="advantage-icon">✓</view>
          <view class="advantage-info">
            <text class="advantage-title">专业定制</text>
            <text class="advantage-desc">支持个性化定制，满足多样需求</text>
          </view>
        </view>
        <view class="advantage-item">
          <view class="advantage-icon">✓</view>
          <view class="advantage-info">
            <text class="advantage-title">品质保证</text>
            <text class="advantage-desc">严格质检，品质有保障</text>
          </view>
        </view>
      </view>
    </view>

    <!-- 联系我们 -->
    <view class="contact-section">
      <view class="section-title">联系我们</view>
      <view class="contact-list">
        <view class="contact-item" @tap="copyText('400-888-8888')">
          <text class="contact-label">客服电话</text>
          <text class="contact-value">400-888-8888</text>
        </view>
        <view class="contact-item" @tap="copyText('service@rongtieyitong.com')">
          <text class="contact-label">电子邮箱</text>
          <text class="contact-value">service@rongtieyitong.com</text>
        </view>
        <view class="contact-item">
          <text class="contact-label">公司地址</text>
          <text class="contact-value">广东省深圳市南山区科技园</text>
        </view>
        <view class="contact-item">
          <text class="contact-label">工作时间</text>
          <text class="contact-value">周一至周日 9:00-21:00</text>
        </view>
      </view>
    </view>

    <!-- 版权信息 -->
    <view class="footer">
      <text class="footer-text">© 2024 溶贴易通 版权所有</text>
      <text class="footer-text">粤ICP备XXXXXXXX号</text>
    </view>
  </view>
</template>

<script>
export default {
  methods: {
    goBack() {
      uni.navigateBack()
    },
    copyText(text) {
      uni.setClipboardData({
        data: text,
        success: () => {
          uni.showToast({ title: '已复制', icon: 'success' })
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
}

/* 品牌信息 */
.brand-section {
  margin: 24rpx 36rpx;
  padding: 48rpx 36rpx;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 16rpx;
  background: rgba(255, 255, 255, 0.95);
  border-radius: 20rpx;
  box-shadow: 0 2rpx 12rpx rgba(0, 0, 0, 0.08);
}

.brand-logo {
  width: 160rpx;
  height: 160rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 36rpx;
  color: #ffffff;
  background: linear-gradient(135deg, #66bb6a, #43a047);
  border-radius: 32rpx;
}

.brand-name {
  font-size: 40rpx;
  font-weight: 700;
  color: #333333;
}

.brand-slogan {
  font-size: 28rpx;
  color: #666666;
}

.brand-version {
  font-size: 24rpx;
  color: #999999;
  margin-top: 8rpx;
}

/* 公司简介 */
.intro-section {
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

.intro-text {
  font-size: 28rpx;
  color: #666666;
  line-height: 1.8;
  margin-bottom: 16rpx;
  display: block;
}

/* 核心优势 */
.advantage-section {
  margin: 0 36rpx 24rpx;
  padding: 32rpx 36rpx;
  background: rgba(255, 255, 255, 0.95);
  border-radius: 20rpx;
  box-shadow: 0 2rpx 12rpx rgba(0, 0, 0, 0.08);
}

.advantage-list {
  display: flex;
  flex-direction: column;
  gap: 24rpx;
}

.advantage-item {
  display: flex;
  flex-direction: row;
  align-items: center;
  gap: 20rpx;
}

.advantage-icon {
  width: 48rpx;
  height: 48rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 28rpx;
  color: #ffffff;
  background: #66bb6a;
  border-radius: 50%;
  flex-shrink: 0;
}

.advantage-info {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 8rpx;
}

.advantage-title {
  font-size: 30rpx;
  font-weight: 600;
  color: #333333;
}

.advantage-desc {
  font-size: 26rpx;
  color: #666666;
}

/* 联系我们 */
.contact-section {
  margin: 0 36rpx 24rpx;
  padding: 32rpx 36rpx;
  background: rgba(255, 255, 255, 0.95);
  border-radius: 20rpx;
  box-shadow: 0 2rpx 12rpx rgba(0, 0, 0, 0.08);
}

.contact-list {
  display: flex;
  flex-direction: column;
}

.contact-item {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  padding: 24rpx 0;
  border-bottom: 1rpx solid rgba(0, 0, 0, 0.06);
}

.contact-item:last-child {
  border-bottom: none;
}

.contact-label {
  font-size: 28rpx;
  color: #666666;
}

.contact-value {
  font-size: 28rpx;
  color: #333333;
  font-weight: 500;
}

/* 版权信息 */
.footer {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 12rpx;
  padding: 48rpx 36rpx;
}

.footer-text {
  font-size: 24rpx;
  color: #999999;
}
</style>