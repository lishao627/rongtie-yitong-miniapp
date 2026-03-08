<template>
  <!-- 首页：简约科技风，与登录页统一视觉 -->
  <view class="page">
    <!-- 状态栏占位 -->
    <view class="status-bar"></view>
    
    <!-- 顶栏 -->
    <view class="header">
      <view class="header-logo-wrap header-content">
        <image class="header-logo" src="/static/logo.png" mode="aspectFit" />
      </view>
      <text class="header-title header-content">溶贴易通</text>
      <view class="header-menu header-content" @tap="onMenu">
        <view class="menu-line" />
        <view class="menu-line" />
        <view class="menu-line" />
      </view>
    </view>

    <!-- 搜索栏 -->
    <view class="search-section">
      <view class="search-bar glass-card">
        <view class="search-icon glass-card-content"></view>
        <input 
          class="search-input glass-card-content" 
          v-model="searchKeyword"
          placeholder="搜索可溶性商标产品..." 
          placeholder-class="search-placeholder"
          @confirm="onSearch"
          @input="onSearchInput"
        />
        <view v-if="searchKeyword" class="search-clear glass-card-content" @tap="clearSearch">×</view>
      </view>
    </view>

    <!-- 搜索结果 -->
    <view v-if="isSearching" class="section">
      <view class="section-header">
        <text class="section-title">搜索结果</text>
        <text class="section-more" @tap="clearSearch">返回</text>
      </view>
      <view v-if="searchResults.length > 0" class="product-list">
        <view 
          v-for="item in searchResults" 
          :key="item.id"
          class="product-card glass-card"
          @tap="goProductDetail(item.id)"
        >
          <view class="product-image-wrap glass-card-content">
            <image class="product-image" :src="item.image" mode="aspectFill" />
          </view>
          <view class="product-info glass-card-content">
            <text class="product-name">{{ item.name }}</text>
            <text class="product-price">¥{{ item.price }}</text>
          </view>
        </view>
      </view>
      <view v-else class="empty-result glass-card">
        <text class="empty-text">未找到相关商品</text>
        <text class="empty-hint">试试其他关键词</text>
      </view>
    </view>

    <!-- 推荐商品 -->
    <view v-else class="section">
      <view class="section-header">
        <text class="section-title">推荐商品</text>
        <text class="section-more" @tap="goProductList('all')">全部</text>
      </view>
      <view class="product-list">
        <view 
          v-for="item in recommendedProducts" 
          :key="item.id"
          class="product-card glass-card"
          @tap="goProductDetail(item.id)"
        >
          <view class="product-image-wrap glass-card-content">
            <image class="product-image" :src="item.image" mode="aspectFill" />
          </view>
          <view class="product-info glass-card-content">
            <text class="product-name">{{ item.name }}</text>
            <text class="product-price">¥{{ item.price }}</text>
          </view>
        </view>
      </view>
    </view>

    <!-- 功能导航 -->
    <view v-if="!isSearching" class="section">
      <view class="grid four">
        <view class="glass-card card-tap interactive card-hover" @tap="goProductList('all')">
          <text class="card-title glass-card-content">全部商品</text>
        </view>
        <view class="glass-card card-tap interactive card-hover" @tap="onCouponCenter">
          <text class="card-title glass-card-content">领券中心</text>
        </view>
        <view class="glass-card card-tap interactive card-hover" @tap="goCart">
          <text class="card-title glass-card-content">购物车</text>
        </view>
        <view class="glass-card card-tap interactive card-hover" @tap="goMine">
          <text class="card-title glass-card-content">个人中心</text>
        </view>
      </view>
    </view>


  </view>
</template>

<script>
export default {
  data() {
    return {
      searchKeyword: '',
      isSearching: false,
      searchResults: [],
      // 所有商品数据
      allProducts: [
        {
          id: 1,
          name: '苹果可溶性商标',
          price: 19.9,
          image: '/static/apple.jpg'
        },
        {
          id: 2,
          name: '梨可溶性商标',
          price: 21.9,
          image: '/static/梨子.jpg'
        },
        {
          id: 3,
          name: '草莓可溶性商标',
          price: 28.9,
          image: '/static/草莓.jpg'
        },
        {
          id: 4,
          name: '蓝莓可溶性商标',
          price: 32.9,
          image: '/static/蓝莓.jpg'
        },
        {
          id: 5,
          name: '葡萄可溶性商标',
          price: 26.9,
          image: '/static/grape.jpg'
        },
        {
          id: 6,
          name: '生菜可溶性商标',
          price: 18.9,
          image: '/static/定制蔬菜.jpg'
        },
        {
          id: 7,
          name: '西红柿可溶性商标',
          price: 20.9,
          image: '/static/tomato.jpg'
        },
        {
          id: 8,
          name: '定制水果商标',
          price: 29.9,
          image: '/static/定制水果.jpg'
        },
        {
          id: 9,
          name: '定制蔬菜商标',
          price: 27.9,
          image: '/static/定制蔬菜.jpg'
        },
        {
          id: 10,
          name: '企业定制商标',
          price: 39.9,
          image: '/static/企业定制.png'
        }
      ],
      recommendedProducts: [
        {
          id: 8,
          name: '企业定制商标',
          price: 39.9,
          image: '/static/企业定制.png'
        },
        {
          id: 1,
          name: '苹果可溶性商标',
          price: 19.9,
          image: '/static/apple.jpg'
        },
        {
          id: 2,
          name: '梨可溶性商标',
          price: 21.9,
          image: '/static/梨子.jpg'
        },
        {
          id: 3,
          name: '草莓可溶性商标',
          price: 28.9,
          image: '/static/草莓.jpg'
        },
        {
          id: 4,
          name: '蓝莓可溶性商标',
          price: 32.9,
          image: '/static/蓝莓.jpg'
        },
        {
          id: 5,
          name: '葡萄可溶性商标',
          price: 26.9,
          image: '/static/grape.jpg'
        }
      ]
    }
  },
  methods: {
    onMenu() {
      uni.showToast({ title: '菜单功能开发中', icon: 'none' })
    },
    onSearchInput() {
      if (this.searchKeyword.trim()) {
        this.isSearching = true
        this.performSearch()
      } else {
        this.isSearching = false
        this.searchResults = []
      }
    },
    onSearch() {
      if (!this.searchKeyword.trim()) {
        uni.showToast({ title: '请输入搜索关键词', icon: 'none' })
        return
      }
      this.isSearching = true
      this.performSearch()
    },
    performSearch() {
      const keyword = this.searchKeyword.trim().toLowerCase()
      this.searchResults = this.allProducts.filter(product => {
        return product.name.toLowerCase().includes(keyword)
      })
    },
    clearSearch() {
      this.searchKeyword = ''
      this.isSearching = false
      this.searchResults = []
    },
    goProductDetail(id) {
      uni.navigateTo({ url: '/pages/product/detail?id=' + id })
    },
    goProductList(type) {
      uni.navigateTo({ url: '/pages/shop/list?type=' + type })
    },
    onCouponCenter() {
      uni.navigateTo({ url: '/pages/coupons/coupons' })
    },
    goCart() {
      uni.switchTab({ url: '/pages/cart/cart' })
    },
    goMine() {
      uni.switchTab({ url: '/pages/mine/mine' })
    }
  }
}
</script>

<style>
/* 与登录页统一：全屏渐变 - 自然绿色系 */
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

/* 顶栏：简约 */
.header {
  height: 110rpx;
  padding: 0 36rpx;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
}

.header-content {
  position: relative;
  z-index: 1;
}

.header-logo-wrap {
  width: 64rpx;
  height: 64rpx;
  border-radius: 16rpx;
  overflow: hidden;
}

.header-logo {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.header-title {
  font-size: 36rpx;
  font-weight: 700;
  color: #666666;
  letter-spacing: 2rpx;
}

.header-menu {
  width: 48rpx;
  height: 48rpx;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 8rpx;
}

.menu-line {
  width: 36rpx;
  height: 4rpx;
  background: #666666;
  border-radius: 2rpx;
}

/* 状态栏占位 */
.status-bar {
  height: var(--status-bar-height);
  width: 100%;
}

/* 搜索栏 */
.search-section {
  padding: 20rpx 36rpx 30rpx;
}

.search-bar {
  height: 88rpx;
  border-radius: 44rpx;
  display: flex;
  flex-direction: row;
  align-items: center;
  padding: 0 32rpx;
  position: relative;
  overflow: hidden;
}

.search-bar::before {
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
  border-radius: 44rpx;
  z-index: 0;
}

.glass-card-content {
  position: relative;
  z-index: 1;
}

.search-icon {
  width: 40rpx;
  height: 40rpx;
  margin-right: 16rpx;
  background: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="%23999" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="11" cy="11" r="8"/><path d="m21 21-4.35-4.35"/></svg>') center/contain no-repeat;
}

.search-input {
  flex: 1;
  font-size: 28rpx;
  color: #666666;
  height: 100%;
  line-height: 88rpx;
}

.search-placeholder {
  color: rgba(0, 0, 0, 0.3);
}

.search-clear {
  width: 40rpx;
  height: 40rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 32rpx;
  color: #999999;
  margin-left: 16rpx;
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

/* 内容区通用 */
.section {
  margin: 0 36rpx 30rpx;
}

.section-header {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 24rpx;
}

.section-title {
  font-size: 34rpx;
  font-weight: 700;
  color: #666666;
}

.section-more {
  font-size: 36rpx;
  color: #66bb6a;
  font-weight: 700;
}

/* 推荐商品：两列布局 */
.product-list {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  gap: 20rpx;
}

.product-card {
  width: calc(50% - 10rpx);
  padding: 16rpx;
  display: flex;
  flex-direction: column;
  gap: 12rpx;
  box-sizing: border-box;
}

.product-image-wrap {
  width: 100%;
  height: 200rpx;
  border-radius: 12rpx;
  overflow: hidden;
}

.product-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.product-info {
  display: flex;
  flex-direction: column;
  gap: 8rpx;
}

.product-name {
  font-size: 26rpx;
  color: #666666;
  font-weight: 600;
  text-align: center;
  line-height: 1.3;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
  min-height: 68rpx;
}

.product-price {
  font-size: 30rpx;
  color: #66bb6a;
  font-weight: 700;
  text-align: center;
}

/* 搜索结果为空 */
.empty-result {
  padding: 80rpx 36rpx;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 16rpx;
}

.empty-text {
  font-size: 32rpx;
  color: #666666;
  font-weight: 600;
}

.empty-hint {
  font-size: 26rpx;
  color: #999999;
}

/* 功能导航 */
.grid {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  gap: 20rpx;
}

.grid.four .glass-card {
  width: calc(25% - 15rpx);
  aspect-ratio: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20rpx;
}

.card-title {
  font-size: 26rpx;
  color: #666666;
  font-weight: 600;
  text-align: center;
}

/* 交互反馈 */
.interactive {
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.interactive:active {
  transform: scale(0.96);
}

.card-hover:hover {
  box-shadow: 0 8rpx 24rpx rgba(0, 0, 0, 0.08);
}

.card-tap {
  cursor: pointer;
}
</style>
