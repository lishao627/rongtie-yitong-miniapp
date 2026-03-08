<template>
  <!-- 商品详情：延续登录页风格，展示可溶性商标产品详细信息 -->
  <view class="page">
    <!-- 顶部导航栏 -->
    <view class="header glass-card">
      <view class="header-left" @tap="goBack">
        <text class="header-icon">←</text>
      </view>
      <text class="header-title">商品详情</text>
      <view class="header-right" @tap="onShare">
        <text class="header-icon">分享</text>
      </view>
    </view>

    <!-- 上方：商品图 -->
    <view class="image-placeholder">
      <image
        v-if="detail.image"
        class="product-image"
        :src="detail.image"
        mode="aspectFill"
      />
      <view v-else class="placeholder-box">
        <text class="placeholder-text">商品图片</text>
        <text class="placeholder-hint">此处预留图片位置</text>
      </view>
    </view>

    <!-- 名称与介绍：玻璃卡片 -->
    <view class="content-wrap">
      <!-- 商品基本信息 -->
      <view class="glass-card">
        <view class="product-name">{{ detail.name }}</view>
        <view class="product-price">¥{{ detail.price }}</view>
        <view class="product-stock">库存: {{ detail.stock }}件</view>
        <view class="product-intro">
          <text class="intro-label">商品介绍</text>
          <text class="intro-text">{{ detail.description }}</text>
        </view>
        <view class="product-features">
          <text class="feature-item">✓食品级安全</text>
          <text class="feature-item">✓环保可降解</text>
          <text class="feature-item">✓专业定制</text>
          <text class="feature-item">✓全国配送</text>
        </view>
      </view>

      <!-- 定制选项 -->
      <view class="glass-card">
        <text class="option-title">定制选项</text>
        <view class="option-group">
          <text class="option-label">尺寸选择</text>
          <view class="option-items">
            <view 
              v-for="size in sizes" 
              :key="size.value"
              class="option-item"
              :class="{ active: selectedSize === size.value }"
              @tap="selectSize(size.value)"
            >
              {{ size.label }}
            </view>
          </view>
        </view>
        <view class="option-group">
          <text class="option-label">材质选择</text>
          <view class="option-items">
            <view 
              v-for="material in materials" 
              :key="material.value"
              class="option-item"
              :class="{ active: selectedMaterial === material.value }"
              @tap="selectMaterial(material.value)"
            >
              {{ material.label }}
            </view>
          </view>
        </view>
        <view class="option-group">
          <text class="option-label">数量选择</text>
          <view class="quantity-options">
            <view 
              v-for="opt in quantityOptions" 
              :key="opt.value"
              class="quantity-option"
              :class="{ active: quantity === opt.value }"
              @tap="selectQuantity(opt.value)"
            >
              {{ opt.label }}
            </view>
          </view>
          <view v-if="quantity === 'custom'" class="custom-quantity">
            <input 
              class="custom-input" 
              type="number" 
              v-model="customQuantity" 
              placeholder="输入具体数量"
              @input="onCustomQuantityChange"
            />
          </view>
        </view>
        <!-- 定制图片上传（仅定制类商品显示） -->
        <view v-if="isCustomProduct" class="option-group">
          <text class="option-label">上传定制图片</text>
          <view class="upload-section">
            <view v-if="customImage" class="uploaded-image">
              <image :src="customImage" mode="aspectFill" class="preview-image" />
              <view class="delete-image" @tap="deleteImage">×</view>
            </view>
            <view v-else class="upload-btn" @tap="uploadImage">
              <text class="upload-icon">+</text>
              <text class="upload-text">点击上传图片</text>
            </view>
          </view>
          <text class="upload-tip">支持上传企业LOGO或定制图案</text>
        </view>
      </view>

      <!-- 商品详情 -->
      <view class="glass-card">
        <text class="detail-title">商品详情</text>
        <view class="detail-content">
          <text class="detail-text">溶贴易通专业生产可溶性商标，采用食品级材料制作，环保可降解，安全无害。</text>
          <text class="detail-text">我们的商标可以定制各种尺寸和材质，适用于各种水果、蔬菜等农产品的标识需求。</text>
          <text class="detail-text">所有产品均通过严格的质量检测，确保符合国家相关标准，让您使用更放心。</text>
        </view>
      </view>

      <!-- 评论区 -->
      <view class="glass-card comment-section">
        <view class="comment-title">用户评价</view>
        <view
          v-for="(c, i) in comments"
          :key="i"
          class="comment-item"
        >
          <view class="comment-header">
            <text class="comment-user">{{ c.user }}</text>
            <text class="comment-star">{{ c.star }}</text>
          </view>
          <text class="comment-text">{{ c.text }}</text>
          <text class="comment-time">{{ c.time }}</text>
        </view>
      </view>
    </view>

    <!-- 底部操作栏 -->
    <view class="bottom-bar">
      <view class="bottom-content">
        <view class="cart-icon" @tap="goCart">
          <text class="cart-symbol">🛒</text>
          <view v-if="cartCount > 0" class="cart-badge">{{ cartCount }}</view>
        </view>
        <view class="bottom-actions">
          <button class="btn-cart" type="default" @tap="onAddCart">加入购物车</button>
          <button class="btn-buy" type="default" @tap="onBuy">立即购买</button>
        </view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      detail: {
        id: '',
        name: '',
        description: '',
        price: '',
        stock: '',
        image: '',
      },
      comments: [
        { user: '用户A', star: '★★★★★', text: '质量很好，回购多次', time: '2024-01-15' },
        { user: '用户B', star: '★★★★★', text: '性价比高，推荐', time: '2024-01-10' },
        { user: '用户C', star: '★★★★★', text: '和描述一致，满意', time: '2024-01-08' },
      ],
      sizes: [
        { label: '小尺寸', value: 'small' },
        { label: '中尺寸', value: 'medium' },
        { label: '大尺寸', value: 'large' },
      ],
      materials: [
        { label: '标准材质', value: 'standard' },
        { label: '高级材质', value: 'premium' },
      ],
      selectedSize: 'medium',
      selectedMaterial: 'standard',
      quantity: 1,
      quantityOptions: [
        { label: '1个', value: 1 },
        { label: '10个', value: 10 },
        { label: '100个', value: 100 },
        { label: '具体数量', value: 'custom' }
      ],
      customQuantity: '',
      customImage: '',
      isCustomProduct: false,
      cartCount: 0,
    }
  },
  onLoad(options) {
    if (options.id) {
      this.detail.id = options.id
      // 根据ID加载商品详情
      this.loadProductDetail(options.id)
      // 判断是否为定制类商品（id为6、7、8的是定制商品）
      this.isCustomProduct = ['6', '7', '8'].includes(options.id)
    }
  },
  methods: {
    loadProductDetail(id) {
      // 模拟加载商品详情
      const products = [
        {
          id: '1',
          name: '苹果可溶性商标',
          description: '食品级安全，环保可降解，专业定制',
          price: '19.9',
          stock: '996',
          image: '/static/apple.jpg'
        },
        {
          id: '2',
          name: '梨可溶性商标',
          description: '食品级安全，环保可降解，专业定制',
          price: '21.9',
          stock: '996',
          image: '/static/梨子.jpg'
        },
        {
          id: '3',
          name: '草莓可溶性商标',
          description: '食品级安全，环保可降解，专业定制',
          price: '28.9',
          stock: '997',
          image: '/static/草莓.jpg'
        },
        {
          id: '10',
          name: '蓝莓可溶性商标',
          description: '食品级安全，环保可降解，专业定制',
          price: '32.9',
          stock: '994',
          image: '/static/蓝莓.jpg'
        },
        {
          id: '9',
          name: '葡萄可溶性商标',
          description: '食品级安全，环保可降解，专业定制',
          price: '26.9',
          stock: '995',
          image: '/static/grape.jpg'
        },
        {
          id: '4',
          name: '生菜可溶性商标',
          description: '食品级安全，环保可降解，专业定制',
          price: '18.9',
          stock: '998',
          image: '/static/定制蔬菜.jpg'
        },
        {
          id: '5',
          name: '西红柿可溶性商标',
          description: '食品级安全，环保可降解，专业定制',
          price: '20.9',
          stock: '995',
          image: '/static/tomato.jpg'
        },
        {
          id: '6',
          name: '定制水果商标',
          description: '根据客户需求定制，食品级安全',
          price: '29.9',
          stock: '100',
          image: '/static/定制水果.jpg'
        },
        {
          id: '7',
          name: '定制蔬菜商标',
          description: '根据客户需求定制，食品级安全',
          price: '27.9',
          stock: '100',
          image: '/static/定制蔬菜.jpg'
        },
        {
          id: '8',
          name: '企业定制商标',
          description: '根据企业LOGO和需求定制',
          price: '39.9',
          stock: '50',
          image: '/static/企业定制.png'
        },
      ]
      
      const product = products.find(p => p.id === id)
      if (product) {
        this.detail = product
      }
    },
    goBack() {
      uni.navigateBack()
    },
    onShare() {
      uni.showToast({ title: '分享功能开发中', icon: 'none' })
    },
    selectSize(size) {
      this.selectedSize = size
    },
    selectMaterial(material) {
      this.selectedMaterial = material
    },
    selectQuantity(value) {
      this.quantity = value
      if (value !== 'custom') {
        this.customQuantity = ''
      }
    },
    onCustomQuantityChange(e) {
      const value = parseInt(e.detail.value)
      if (value > 0) {
        this.quantity = value
      }
    },
    uploadImage() {
      uni.chooseImage({
        count: 1,
        sizeType: ['compressed'],
        sourceType: ['album', 'camera'],
        success: (res) => {
          this.customImage = res.tempFilePaths[0]
          uni.showToast({ title: '图片上传成功', icon: 'success' })
        }
      })
    },
    deleteImage() {
      this.customImage = ''
    },
    onBuy() {
      // 立即购买 - 跳转到支付页面
      const finalQuantity = this.quantity === 'custom' ? (parseInt(this.customQuantity) || 1) : this.quantity
      const totalPrice = (parseFloat(this.detail.price) * finalQuantity).toFixed(2)
      uni.navigateTo({ 
        url: '/pages/orders/payment?id=' + this.detail.id + '&price=' + totalPrice + '&quantity=' + finalQuantity
      })
    },
    onAddCart() {
      // 加入购物车
      this.cartCount++
      uni.showToast({ title: '已加入购物车', icon: 'success' })
    },
    goCart() {
      uni.navigateTo({ url: '/pages/cart/cart' })
    },
  },
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
  background: rgba(255, 255, 255, 0.78);
  border-bottom: 1rpx solid rgba(255, 255, 255, 0.5);
  backdrop-filter: blur(12px);
  box-shadow: 0 2rpx 12rpx rgba(0, 0, 0, 0.04), 0 4rpx 20rpx rgba(0, 0, 0, 0.03);
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
  font-size: 36rpx;
  color: #333333;
  font-weight: bold;
}
.header-title {
  font-size: 32rpx;
  font-weight: 600;
  color: #333333;
}

/* 商品图占位 */
.image-placeholder {
  width: 100%;
  height: 480rpx;
  background: rgba(255, 255, 255, 0.3);
  border-bottom: 1rpx solid rgba(255, 255, 255, 0.4);
}

.product-image {
  width: 100%;
  height: 100%;
  display: block;
}

.placeholder-box {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 12rpx;
}

.placeholder-text {
  font-size: 32rpx;
  color: #666666;
}

.placeholder-hint {
  font-size: 24rpx;
  color: #999999;
}

/* 内容区 */
.content-wrap {
  padding: 24rpx 36rpx;
  display: flex;
  flex-direction: column;
  gap: 24rpx;
}

.glass-card {
  width: 100%;
  border-radius: 24rpx;
  padding: 32rpx 28rpx;
  background: rgba(255, 255, 255, 0.78);
  border: 1rpx solid rgba(255, 255, 255, 0.5);
  backdrop-filter: blur(12px);
  box-shadow: 0 2rpx 12rpx rgba(0, 0, 0, 0.04), 0 4rpx 20rpx rgba(0, 0, 0, 0.03);
}

.product-name {
  font-size: 36rpx;
  font-weight: 600;
  color: #333333;
  margin-bottom: 12rpx;
}

.product-price {
  font-size: 32rpx;
  font-weight: 600;
  color: #66bb6a;
  margin-bottom: 12rpx;
}

.product-stock {
  font-size: 24rpx;
  color: #999999;
  margin-bottom: 24rpx;
}

.product-intro {
  margin-bottom: 28rpx;
}

.intro-label {
  font-size: 26rpx;
  color: #7e8496;
  display: block;
  margin-bottom: 12rpx;
}

.intro-text {
  font-size: 28rpx;
  color: #333333;
  line-height: 1.5;
}

/* 产品特点 */
.product-features {
  display: flex;
  flex-wrap: wrap;
  gap: 16rpx;
  margin-top: 20rpx;
}

.feature-item {
  font-size: 24rpx;
  color: #66bb6a;
  background: rgba(102, 187, 106, 0.1);
  padding: 8rpx 16rpx;
  border-radius: 16rpx;
}

/* 定制选项 */
.option-title {
  font-size: 32rpx;
  font-weight: 600;
  color: #333333;
  margin-bottom: 24rpx;
}

.option-group {
  margin-bottom: 28rpx;
}

.option-label {
  font-size: 26rpx;
  color: #7e8496;
  display: block;
  margin-bottom: 16rpx;
}

.option-items {
  display: flex;
  flex-wrap: wrap;
  gap: 16rpx;
}

.option-item {
  font-size: 26rpx;
  color: #333333;
  background: rgba(255, 255, 255, 0.9);
  border: 1rpx solid rgba(0, 0, 0, 0.1);
  padding: 12rpx 24rpx;
  border-radius: 16rpx;
}

.option-item.active {
  color: #66bb6a;
  border-color: #66bb6a;
  background: rgba(102, 187, 106, 0.1);
}

/* 数量选项 */
.quantity-options {
  display: flex;
  flex-wrap: wrap;
  gap: 16rpx;
}

.quantity-option {
  font-size: 26rpx;
  color: #333333;
  background: rgba(255, 255, 255, 0.9);
  border: 1rpx solid rgba(0, 0, 0, 0.1);
  padding: 12rpx 24rpx;
  border-radius: 16rpx;
}

.quantity-option.active {
  color: #66bb6a;
  border-color: #66bb6a;
  background: rgba(102, 187, 106, 0.1);
}

.custom-quantity {
  margin-top: 16rpx;
}

.custom-input {
  width: 100%;
  height: 80rpx;
  padding: 0 24rpx;
  font-size: 28rpx;
  color: #333333;
  background: rgba(255, 255, 255, 0.9);
  border: 1rpx solid rgba(0, 0, 0, 0.1);
  border-radius: 16rpx;
  box-sizing: border-box;
}

/* 图片上传 */
.upload-section {
  margin-top: 16rpx;
}

.upload-btn {
  width: 200rpx;
  height: 200rpx;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 16rpx;
  background: rgba(255, 255, 255, 0.9);
  border: 2rpx dashed rgba(102, 187, 106, 0.5);
  border-radius: 16rpx;
}

.upload-icon {
  font-size: 48rpx;
  color: #66bb6a;
}

.upload-text {
  font-size: 24rpx;
  color: #66bb6a;
}

.uploaded-image {
  width: 200rpx;
  height: 200rpx;
  position: relative;
  border-radius: 16rpx;
  overflow: hidden;
}

.preview-image {
  width: 100%;
  height: 100%;
}

.delete-image {
  position: absolute;
  top: 8rpx;
  right: 8rpx;
  width: 40rpx;
  height: 40rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 28rpx;
  color: #ffffff;
  background: rgba(255, 0, 0, 0.8);
  border-radius: 50%;
}

.upload-tip {
  font-size: 22rpx;
  color: #999999;
  margin-top: 12rpx;
  display: block;
}

/* 商品详情 */
.detail-title {
  font-size: 32rpx;
  font-weight: 600;
  color: #333333;
  margin-bottom: 24rpx;
}

.detail-content {
  display: flex;
  flex-direction: column;
  gap: 16rpx;
}

.detail-text {
  font-size: 26rpx;
  color: #666666;
  line-height: 1.5;
}

/* 评论区 */
.comment-section {
  margin-top: 0;
}

.comment-title {
  font-size: 30rpx;
  font-weight: 600;
  color: #333333;
  margin-bottom: 24rpx;
}

.comment-item {
  padding: 20rpx 0;
  border-bottom: 1rpx solid rgba(0, 0, 0, 0.06);
}

.comment-item:last-child {
  border-bottom: none;
}

.comment-header {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 8rpx;
}

.comment-user {
  font-size: 26rpx;
  font-weight: 600;
  color: #333333;
}

.comment-star {
  font-size: 24rpx;
  color: #f5a623;
}

.comment-text {
  font-size: 26rpx;
  color: #666666;
  line-height: 1.5;
  display: block;
  margin-bottom: 8rpx;
}

.comment-time {
  font-size: 22rpx;
  color: #999999;
}

/* 底部操作栏 */
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

.cart-icon {
  position: relative;
  font-size: 48rpx;
}

.cart-symbol {
  font-size: 48rpx;
}

.cart-badge {
  position: absolute;
  top: -8rpx;
  right: -8rpx;
  min-width: 32rpx;
  height: 32rpx;
  line-height: 32rpx;
  text-align: center;
  font-size: 20rpx;
  color: #fff;
  background: #66bb6a;
  border-radius: 16rpx;
  padding: 0 8rpx;
}

.bottom-actions {
  display: flex;
  flex-direction: row;
  gap: 16rpx;
}

.btn-buy,
.btn-cart {
  height: 72rpx;
  line-height: 72rpx;
  border-radius: 36rpx;
  font-size: 28rpx;
  font-weight: 500;
  border: none;
  padding: 0 28rpx;
  margin: 0;
}

.btn-buy {
  color: #ffffff;
  background: linear-gradient(90deg, #66bb6a, #43a047);
}

.btn-cart {
  color: #66bb6a;
  background: rgba(102, 187, 106, 0.1);
  border: 1rpx solid #66bb6a;
}

.btn-buy::after,
.btn-cart::after {
  border: none;
}
</style>
