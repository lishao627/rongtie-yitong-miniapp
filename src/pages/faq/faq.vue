<template>
  <!-- 常见问题页面：延续小程序设计风格 -->
  <view class="page">
    <!-- 顶部导航栏 -->
    <view class="header">
      <view class="header-left" @tap="goBack">
        <text class="header-icon">←</text>
      </view>
      <text class="header-title">常见问题</text>
      <view class="header-right"></view>
    </view>

    <!-- 问题分类 -->
    <view class="category-bar">
      <view 
        v-for="category in categories" 
        :key="category.value"
        class="category-item"
        :class="{ active: currentCategory === category.value }"
        @tap="switchCategory(category.value)"
      >
        {{ category.label }}
      </view>
    </view>

    <!-- FAQ列表 -->
    <view class="content-wrap">
      <view 
        v-for="(faq, index) in filteredFaqs" 
        :key="index"
        class="faq-card"
      >
        <view class="faq-question" @tap="toggleFaq(index)">
          <view class="question-icon">Q</view>
          <text class="question-text">{{ faq.question }}</text>
          <text class="question-arrow" :class="{ expanded: faq.expanded }">▼</text>
        </view>
        <view v-if="faq.expanded" class="faq-answer">
          <view class="answer-icon">A</view>
          <text class="answer-text">{{ faq.answer }}</text>
        </view>
      </view>
    </view>

    <!-- 底部联系客服 -->
    <view class="bottom-bar">
      <view class="bottom-content">
        <text class="bottom-text">没找到答案？</text>
        <button class="btn-contact" type="default" @tap="goService">联系客服</button>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      currentCategory: 'all',
      categories: [
        { label: '全部', value: 'all' },
        { label: '购物指南', value: 'shopping' },
        { label: '订单问题', value: 'order' },
        { label: '售后服务', value: 'service' },
        { label: '账户相关', value: 'account' }
      ],
      faqs: [
        {
          category: 'shopping',
          question: '如何下单购买可溶性商标？',
          answer: '您可以在商品列表中选择心仪的商品，点击选择规格后加入购物车，然后在购物车中结算。支持微信支付、支付宝等多种支付方式。',
          expanded: false
        },
        {
          category: 'shopping',
          question: '可溶性商标的材质安全吗？',
          answer: '我们的商标采用食品级材料制作，环保可降解，安全无害，符合国家标准。所有产品均通过严格的质量检测，让您使用更放心。',
          expanded: false
        },
        {
          category: 'shopping',
          question: '支持定制吗？如何定制？',
          answer: '支持定制！您可以在定制类商品中选择，或者联系客服进行个性化定制。我们提供企业LOGO定制、特殊尺寸定制等服务。',
          expanded: false
        },
        {
          category: 'order',
          question: '订单发货后多久能收到？',
          answer: '一般情况下，订单发货后3-5个工作日内送达，具体时间视地区而定。您可以在订单详情中查看物流信息。',
          expanded: false
        },
        {
          category: 'order',
          question: '如何查看订单物流信息？',
          answer: '进入"我的订单"页面，点击对应订单进入详情页，即可查看物流信息和配送进度。',
          expanded: false
        },
        {
          category: 'order',
          question: '可以修改订单信息吗？',
          answer: '订单未发货前可以联系客服修改收货地址等信息。订单发货后暂不支持修改，请您在下单时仔细核对信息。',
          expanded: false
        },
        {
          category: 'service',
          question: '如何申请退款/退货？',
          answer: '您可以在订单详情中申请退款/退货，或联系客服协助处理。我们支持7天无理由退货，定制类商品除外。',
          expanded: false
        },
        {
          category: 'service',
          question: '优惠券怎么使用？',
          answer: '在结算页面选择可用的优惠券即可自动抵扣相应金额。每张优惠券有使用条件和有效期，请在使用前仔细阅读。',
          expanded: false
        },
        {
          category: 'service',
          question: '产品质量问题如何处理？',
          answer: '如遇到产品质量问题，请及时联系客服，提供相关照片，我们会第一时间为您处理，支持退换货服务。',
          expanded: false
        },
        {
          category: 'account',
          question: '如何修改个人信息？',
          answer: '进入"我的"页面，点击头像区域即可修改个人信息，包括昵称、头像等。',
          expanded: false
        },
        {
          category: 'account',
          question: '忘记密码怎么办？',
          answer: '在登录页面点击"忘记密码"，按照提示进行密码重置。如有问题可联系客服协助处理。',
          expanded: false
        },
        {
          category: 'account',
          question: '如何绑定手机号？',
          answer: '进入"我的"页面，在设置中进行手机号绑定。绑定手机号后可以享受更多服务和优惠。',
          expanded: false
        }
      ]
    }
  },
  computed: {
    filteredFaqs() {
      if (this.currentCategory === 'all') {
        return this.faqs
      }
      return this.faqs.filter(faq => faq.category === this.currentCategory)
    }
  },
  methods: {
    goBack() {
      uni.navigateBack()
    },
    switchCategory(category) {
      this.currentCategory = category
    },
    toggleFaq(index) {
      const faq = this.filteredFaqs[index]
      if (faq) {
        faq.expanded = !faq.expanded
      }
    },
    goService() {
      uni.navigateTo({ url: '/pages/service/service' })
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

/* 分类标签 */
.category-bar {
  margin: 24rpx 36rpx;
  padding: 16rpx;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  gap: 16rpx;
  background: rgba(255, 255, 255, 0.95);
  border-radius: 16rpx;
  box-shadow: 0 2rpx 12rpx rgba(0, 0, 0, 0.08);
}

.category-item {
  padding: 12rpx 24rpx;
  font-size: 26rpx;
  color: #666666;
  background: rgba(0, 0, 0, 0.05);
  border-radius: 24rpx;
  transition: all 0.3s ease;
}

.category-item.active {
  color: #ffffff;
  background: #66bb6a;
}

/* FAQ列表 */
.content-wrap {
  padding: 0 36rpx;
}

.faq-card {
  margin-bottom: 24rpx;
  background: rgba(255, 255, 255, 0.95);
  border-radius: 20rpx;
  box-shadow: 0 2rpx 12rpx rgba(0, 0, 0, 0.08);
  overflow: hidden;
}

.faq-question {
  display: flex;
  flex-direction: row;
  align-items: center;
  gap: 16rpx;
  padding: 28rpx;
}

.question-icon {
  width: 44rpx;
  height: 44rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 24rpx;
  color: #ffffff;
  background: #66bb6a;
  border-radius: 50%;
  flex-shrink: 0;
}

.question-text {
  flex: 1;
  font-size: 30rpx;
  color: #333333;
  font-weight: 600;
  line-height: 1.4;
}

.question-arrow {
  font-size: 24rpx;
  color: #999999;
  transition: transform 0.3s ease;
}

.question-arrow.expanded {
  transform: rotate(180deg);
}

.faq-answer {
  display: flex;
  flex-direction: row;
  gap: 16rpx;
  padding: 0 28rpx 28rpx;
  border-top: 1rpx solid rgba(0, 0, 0, 0.06);
  margin-top: 0;
  padding-top: 28rpx;
}

.answer-icon {
  width: 44rpx;
  height: 44rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 24rpx;
  color: #ffffff;
  background: #999999;
  border-radius: 50%;
  flex-shrink: 0;
}

.answer-text {
  flex: 1;
  font-size: 28rpx;
  color: #666666;
  line-height: 1.6;
}

/* 底部联系客服 */
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

.bottom-text {
  font-size: 28rpx;
  color: #666666;
}

.btn-contact {
  padding: 0 40rpx;
  height: 64rpx;
  line-height: 64rpx;
  font-size: 28rpx;
  color: #ffffff;
  background: linear-gradient(90deg, #66bb6a, #43a047);
  border-radius: 32rpx;
  border: none;
  margin: 0;
}

.btn-contact::after {
  border: none;
}
</style>