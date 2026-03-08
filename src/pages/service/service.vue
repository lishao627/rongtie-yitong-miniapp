<template>
  <!-- 客服中心页面：延续小程序设计风格 -->
  <view class="page">
    <!-- 顶部导航栏 -->
    <view class="header">
      <view class="header-left" @tap="goBack">
        <text class="header-icon">←</text>
      </view>
      <text class="header-title">客服中心</text>
      <view class="header-right"></view>
    </view>

    <!-- 客服信息 -->
    <view class="service-info">
      <view class="service-avatar">客服</view>
      <text class="service-name">溶贴易通客服</text>
      <text class="service-desc">专业为您解答各类问题</text>
      <view class="service-time">
        <text class="time-text">服务时间：9:00-21:00</text>
      </view>
    </view>

    <!-- 快捷服务 -->
    <view class="quick-service">
      <view class="section-title">快捷服务</view>
      <view class="service-grid">
        <view class="service-item" @tap="contactOnline">
          <view class="service-icon">在线</view>
          <text class="service-text">在线客服</text>
        </view>
        <view class="service-item" @tap="contactPhone">
          <view class="service-icon">电话</view>
          <text class="service-text">电话咨询</text>
        </view>
        <view class="service-item" @tap="contactWechat">
          <view class="service-icon">微信</view>
          <text class="service-text">微信客服</text>
        </view>
        <view class="service-item" @tap="contactEmail">
          <view class="service-icon">邮件</view>
          <text class="service-text">邮件反馈</text>
        </view>
      </view>
    </view>

    <!-- 常见问题 -->
    <view class="faq-section">
      <view class="section-title">常见问题</view>
      <view class="faq-list">
        <view 
          v-for="(faq, index) in faqs" 
          :key="index"
          class="faq-item"
          @tap="showFaqDetail(faq)"
        >
          <text class="faq-question">{{ faq.question }}</text>
          <text class="faq-solution">解决方法</text>
        </view>
      </view>
    </view>

    <!-- 意见反馈 -->
    <view class="feedback-section">
      <view class="section-title">意见反馈</view>
      <view class="feedback-form">
        <textarea 
          class="feedback-input" 
          placeholder="请输入您的意见或建议，我们会认真听取..."
          v-model="feedbackContent"
          maxlength="200"
        />
        <text class="feedback-count">{{ feedbackContent.length }}/200</text>
        <button class="btn-submit" type="default" @tap="submitFeedback">提交反馈</button>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      feedbackContent: '',
      faqs: [
        { question: '如何下单购买可溶性商标？', answer: '您可以在商品列表中选择心仪的商品，点击选择规格后加入购物车，然后在购物车中结算。' },
        { question: '可溶性商标的材质安全吗？', answer: '我们的商标采用食品级材料制作，环保可降解，安全无害，符合国家标准。' },
        { question: '支持定制吗？如何定制？', answer: '支持定制！您可以在定制类商品中选择，或者联系客服进行个性化定制。' },
        { question: '订单发货后多久能收到？', answer: '一般情况下，订单发货后3-5个工作日内送达，具体时间视地区而定。' },
        { question: '如何申请退款/退货？', answer: '您可以在订单详情中申请退款/退货，或联系客服协助处理。' },
        { question: '优惠券怎么使用？', answer: '在结算页面选择可用的优惠券即可自动抵扣相应金额。' }
      ]
    }
  },
  methods: {
    goBack() {
      uni.navigateBack()
    },
    contactOnline() {
      uni.showToast({ title: '在线客服功能开发中', icon: 'none' })
    },
    contactPhone() {
      uni.makePhoneCall({
        phoneNumber: '400-888-8888',
        success: () => {
          console.log('拨打电话成功')
        },
        fail: () => {
          uni.showToast({ title: '拨打电话失败', icon: 'none' })
        }
      })
    },
    contactWechat() {
      uni.showToast({ title: '微信客服：rongtieyitong', icon: 'none' })
    },
    contactEmail() {
      uni.showToast({ title: '邮箱：service@rongtieyitong.com', icon: 'none' })
    },
    showFaqDetail(faq) {
      uni.showModal({
        title: faq.question,
        content: faq.answer,
        showCancel: false
      })
    },
    submitFeedback() {
      if (!this.feedbackContent.trim()) {
        uni.showToast({ title: '请输入反馈内容', icon: 'none' })
        return
      }
      uni.showToast({ title: '反馈提交成功', icon: 'success' })
      this.feedbackContent = ''
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

/* 客服信息 */
.service-info {
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

.service-avatar {
  width: 120rpx;
  height: 120rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 40rpx;
  color: #ffffff;
  background: linear-gradient(135deg, #66bb6a, #43a047);
  border-radius: 50%;
}

.service-name {
  font-size: 36rpx;
  font-weight: 600;
  color: #333333;
}

.service-desc {
  font-size: 26rpx;
  color: #666666;
}

.service-time {
  margin-top: 16rpx;
  padding: 12rpx 32rpx;
  background: rgba(102, 187, 106, 0.1);
  border-radius: 28rpx;
}

.time-text {
  font-size: 24rpx;
  color: #66bb6a;
}

/* 快捷服务 */
.quick-service {
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

.service-grid {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  gap: 20rpx;
}

.service-item {
  width: calc(25% - 15rpx);
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 12rpx;
  padding: 24rpx 0;
}

.service-icon {
  width: 80rpx;
  height: 80rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 28rpx;
  color: #ffffff;
  background: linear-gradient(135deg, #66bb6a, #43a047);
  border-radius: 50%;
}

.service-text {
  font-size: 24rpx;
  color: #666666;
}

/* 常见问题 */
.faq-section {
  margin: 0 36rpx 24rpx;
  padding: 32rpx 36rpx;
  background: rgba(255, 255, 255, 0.95);
  border-radius: 20rpx;
  box-shadow: 0 2rpx 12rpx rgba(0, 0, 0, 0.08);
}

.faq-list {
  display: flex;
  flex-direction: column;
}

.faq-item {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  padding: 24rpx 0;
  border-bottom: 1rpx solid rgba(0, 0, 0, 0.06);
}

.faq-item:last-child {
  border-bottom: none;
}

.faq-question {
  flex: 1;
  font-size: 28rpx;
  color: #333333;
  line-height: 1.4;
}

.faq-solution {
  font-size: 24rpx;
  color: #66bb6a;
  padding: 8rpx 16rpx;
  background: rgba(102, 187, 106, 0.1);
  border-radius: 8rpx;
  margin-left: 16rpx;
  flex-shrink: 0;
}

/* 意见反馈 */
.feedback-section {
  margin: 0 36rpx;
  padding: 32rpx 36rpx;
  background: rgba(255, 255, 255, 0.95);
  border-radius: 20rpx;
  box-shadow: 0 2rpx 12rpx rgba(0, 0, 0, 0.08);
}

.feedback-form {
  display: flex;
  flex-direction: column;
  gap: 16rpx;
}

.feedback-input {
  width: 100%;
  height: 200rpx;
  padding: 24rpx;
  font-size: 28rpx;
  color: #333333;
  background: rgba(0, 0, 0, 0.03);
  border-radius: 16rpx;
  box-sizing: border-box;
}

.feedback-count {
  font-size: 24rpx;
  color: #999999;
  text-align: right;
}

.btn-submit {
  width: 100%;
  height: 80rpx;
  line-height: 80rpx;
  font-size: 32rpx;
  color: #ffffff;
  background: linear-gradient(90deg, #66bb6a, #43a047);
  border-radius: 40rpx;
  border: none;
  margin-top: 16rpx;
}

.btn-submit::after {
  border: none;
}
</style>