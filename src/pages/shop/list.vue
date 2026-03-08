<template>
  <!-- 商品列表页：风格延续登录页，展示可溶性商标产品 -->
  <view class="page">
    <!-- 状态栏占位 -->
    <view class="status-bar-placeholder"></view>
    
    <!-- 顶部导航栏 -->
    <view class="nav-header">
      <view class="nav-left" @tap="goBack">
        <view class="back-btn">
          <text class="nav-icon">←</text>
        </view>
      </view>
      <text class="nav-title">商品列表</text>
      <view class="nav-right"></view>
    </view>

    <!-- 店铺头部 -->
    <view class="store-header">
      <view class="store-left">
        <view class="store-name-row">
          <text class="store-name">溶贴易通</text>
        </view>
        <text class="store-distance">专业定制可溶性商标</text>
        <view class="store-announce">
          <text class="announce-icon">公告</text>
          <text class="announce-text">食品级安全，环保可降解</text>
        </view>
      </view>
      <view class="store-right">
        <view class="segment">
          <view class="segment-item active">邮寄</view>
          <view class="segment-item" @tap="switchDelivery">定制</view>
        </view>
      </view>
    </view>

    <!-- 主体：左侧分类 + 右侧商品列表 -->
    <view class="body-wrap">
      <scroll-view class="sidebar" scroll-y :scroll-top="catScrollTop">
        <view
          v-for="(cat, idx) in categories"
          :key="cat.id"
          class="cat-item"
          :class="{ active: currentCatId === cat.id }"
          @tap="switchCat(cat)"
        >
          {{ cat.name }}
        </view>
      </scroll-view>
      <scroll-view 
        class="product-area" 
        scroll-y 
        :scroll-into-view="scrollIntoView"
        scroll-with-animation
        @scroll="onProductScroll"
      >
        <!-- 只显示当前选中的分类 -->
        <view 
          v-for="cat in filteredCategories" 
          :key="cat.id" 
          :id="'group-' + cat.id"
          class="product-group"
        >
          <view class="group-title">{{ cat.name }}</view>
          <view class="product-grid">
            <view
              v-for="item in cat.list"
              :key="item.id"
              class="product-card"
              @tap="goProductDetail(item.id)"
            >
              <view class="product-pic-wrap">
                <image v-if="item.image" class="product-pic" :src="item.image" mode="aspectFill" />
                <view v-else class="product-pic-placeholder">图</view>
              </view>
              <view class="product-body">
                <text class="product-title">{{ item.name }}</text>
                <text class="product-desc">{{ item.description }}</text>
                <text class="product-stock">库存{{ item.stock }}</text>
                <view class="product-bottom">
                  <text class="product-price">¥{{ item.price }}</text>
                  <view class="product-actions">
                    <button v-if="item.quantity === 0" class="btn-select" type="default" @tap.stop="addCart(item)">选择</button>
                    <view v-else class="stepper">
                      <text class="stepper-btn" @tap.stop="changeQty(item, -1)">-</text>
                      <text class="stepper-num">{{ item.quantity }}</text>
                      <text class="stepper-btn" @tap.stop="changeQty(item, 1)">+</text>
                    </view>
                  </view>
                </view>
              </view>
            </view>
          </view>
        </view>
        <!-- 底部占位，防止被购物车栏遮挡 -->
        <view class="bottom-placeholder"></view>
      </scroll-view>
    </view>

    <!-- 底部购物车栏 -->
    <view class="cart-bar">
      <view class="cart-left" @tap="goCart">
        <view class="cart-icon-wrap">
          <text class="cart-icon">🛒</text>
          <view v-if="cartCount > 0" class="cart-badge">{{ cartCount }}</view>
        </view>
        <view class="cart-total-wrap">
          <text class="cart-total">¥{{ cartTotal }}</text>
          <text class="cart-hint">支持全国配送</text>
        </view>
      </view>
      <button class="btn-checkout" type="default" @tap="onCheckout">去结算</button>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      currentCatId: 'all',
      currentFilter: 'all',
      catScrollTop: 0,
      scrollIntoView: '',
      groupTops: {},
      categories: [
        {
          id: 'all',
          name: '全部商品',
          list: []
        },
        {
          id: 'fruit',
          name: '水果类商标',
          list: [
            { id: '1', name: '苹果可溶性商标', description: '食品级安全，环保可降解', stock: 996, price: '19.9', quantity: 0, image: '/static/apple.jpg' },
            { id: '2', name: '梨可溶性商标', description: '食品级安全，环保可降解', stock: 996, price: '21.9', quantity: 0, image: '/static/梨子.jpg' },
            { id: '3', name: '草莓可溶性商标', description: '食品级安全，环保可降解', stock: 997, price: '28.9', quantity: 0, image: '/static/草莓.jpg' },
            { id: '10', name: '蓝莓可溶性商标', description: '食品级安全，环保可降解', stock: 994, price: '32.9', quantity: 0, image: '/static/蓝莓.jpg' },
            { id: '9', name: '葡萄可溶性商标', description: '食品级安全，环保可降解', stock: 995, price: '26.9', quantity: 0, image: '/static/grape.jpg' },
          ],
        },
        {
          id: 'vegetable',
          name: '蔬菜类商标',
          list: [
            { id: '4', name: '生菜可溶性商标', description: '食品级安全，环保可降解', stock: 998, price: '18.9', quantity: 0, image: '/static/定制蔬菜.jpg' },
            { id: '5', name: '西红柿可溶性商标', description: '食品级安全，环保可降解', stock: 995, price: '20.9', quantity: 0, image: '/static/tomato.jpg' },
          ],
        },
        {
          id: 'custom',
          name: '定制类商标',
          list: [
            { id: '6', name: '定制水果商标', description: '根据客户需求定制', stock: 100, price: '29.9', quantity: 0, image: '/static/定制水果.jpg' },
            { id: '7', name: '定制蔬菜商标', description: '根据客户需求定制', stock: 100, price: '27.9', quantity: 0, image: '/static/定制蔬菜.jpg' },
            { id: '8', name: '企业定制商标', description: '根据企业LOGO定制', stock: 50, price: '39.9', quantity: 0, image: '/static/企业定制.png' },
          ],
        },
      ],
    }
  },
  computed: {
    // 根据当前选中的分类过滤显示的商品
    filteredCategories() {
      if (this.currentCatId === 'all') {
        // 返回所有分类（除了all本身）
        return this.categories.filter(cat => cat.id !== 'all')
      }
      // 只返回选中的分类
      return this.categories.filter(cat => cat.id === this.currentCatId)
    },
    cartCount() {
      let n = 0
      this.categories.forEach((cat) => {
        cat.list.forEach((item) => { n += item.quantity })
      })
      return n
    },
    cartTotal() {
      let t = 0
      this.categories.forEach((cat) => {
        cat.list.forEach((item) => {
          t += item.quantity * parseFloat(item.price)
        })
      })
      return t.toFixed(2)
    },
  },
  onLoad() {
    // 初始化全部商品列表
    this.initAllProducts()
  },
  onReady() {
    this.calcGroupTops()
  },
  methods: {
    // 初始化全部商品列表
    initAllProducts() {
      const allProducts = []
      this.categories.forEach(cat => {
        if (cat.id !== 'all') {
          allProducts.push(...cat.list)
        }
      })
      const allCategory = this.categories.find(cat => cat.id === 'all')
      if (allCategory) {
        allCategory.list = allProducts
      }
    },
    switchDelivery() {
      uni.showToast({ title: '切换定制模式', icon: 'none' })
    },
    switchCat(cat) {
      this.currentCatId = cat.id
      this.scrollIntoView = ''
      // 如果是全部商品，滚动到顶部
      if (cat.id === 'all') {
        this.scrollIntoView = 'group-fruit'
      } else {
        this.scrollIntoView = 'group-' + cat.id
      }
    },
    calcGroupTops() {
      const query = uni.createSelectorQuery().in(this)
      this.categories.forEach(cat => {
        if (cat.id !== 'all') {
          query.select('#group-' + cat.id).boundingClientRect()
        }
      })
      query.exec(res => {
        if (res && res.length > 0) {
          res.forEach((rect, index) => {
            if (rect && this.categories[index + 1]) {
              this.groupTops[this.categories[index + 1].id] = rect.top
            }
          })
        }
      })
    },
    onProductScroll(e) {
      // 只有在显示全部商品时才需要监听滚动
      if (this.currentCatId !== 'all') return
      
      const scrollTop = e.detail.scrollTop
      let currentId = 'fruit'
      const catIds = ['fruit', 'vegetable', 'custom']
      
      for (let i = catIds.length - 1; i >= 0; i--) {
        const catId = catIds[i]
        const groupTop = this.groupTops[catId]
        if (groupTop && scrollTop >= groupTop - 100) {
          currentId = catId
          break
        }
      }
      
      if (currentId !== this.currentCatId) {
        this.currentCatId = currentId
      }
    },
    addCart(item) {
      item.quantity = 1
      uni.showToast({ title: '已添加到购物车', icon: 'success' })
    },
    changeQty(item, delta) {
      item.quantity = Math.max(0, item.quantity + delta)
    },
    goProductDetail(id) {
      uni.navigateTo({ url: '/pages/product/detail?id=' + id })
    },
    goCart() {
      uni.switchTab({ url: '/pages/cart/cart' })
    },
    goBack() {
      uni.navigateBack()
    },
    onCheckout() {
      if (this.cartCount === 0) {
        uni.showToast({ title: '请先选择商品', icon: 'none' })
        return
      }
      uni.navigateTo({ url: '/pages/orders/payment?price=' + this.cartTotal })
    },
  },
}
</script>

<style>
.page {
  min-height: 100vh;
  width: 100%;
  box-sizing: border-box;
  background-image: radial-gradient(circle at 10% 0, #e8f5e9 0, #c8e6c9 40%, #a5d6a7 70%, #81c784 100%);
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
.nav-header {
  height: 88rpx;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  padding: 0 24rpx;
  margin: 10rpx 24rpx 16rpx;
  background: rgba(255, 255, 255, 0.95);
  border-radius: 16rpx;
  box-shadow: 0 2rpx 12rpx rgba(0, 0, 0, 0.1);
  flex-shrink: 0;
}

.nav-left {
  width: 80rpx;
  height: 60rpx;
  display: flex;
  align-items: center;
  justify-content: center;
}

.back-btn {
  width: 60rpx;
  height: 60rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  background: linear-gradient(90deg, #66bb6a, #43a047);
  border-radius: 50%;
  box-shadow: 0 2rpx 8rpx rgba(102, 187, 106, 0.3);
}

.nav-icon {
  font-size: 32rpx;
  color: #ffffff;
  font-weight: 700;
}

.nav-title {
  font-size: 34rpx;
  font-weight: 600;
  color: #333333;
}

.nav-right {
  width: 80rpx;
}

/* 店铺头部 */
.store-header {
  margin: 0 24rpx 16rpx;
  padding: 24rpx 28rpx;
  display: flex;
  flex-direction: row;
  align-items: flex-start;
  justify-content: space-between;
  background: rgba(255, 255, 255, 0.95);
  border-radius: 20rpx;
  box-shadow: 0 2rpx 12rpx rgba(0, 0, 0, 0.08);
  flex-shrink: 0;
}

.store-left { flex: 1; }
.store-name-row { display: flex; align-items: center; gap: 10rpx; margin-bottom: 8rpx; }
.store-name { font-size: 36rpx; font-weight: 700; color: #333333; }
.store-distance { font-size: 26rpx; color: #666666; display: block; margin-bottom: 8rpx; }
.store-announce { display: flex; align-items: center; gap: 10rpx; }
.announce-icon { font-size: 24rpx; color: #66bb6a; }
.announce-text { font-size: 24rpx; color: #666666; }
.store-right { display: flex; flex-direction: column; align-items: flex-end; gap: 18rpx; }
.segment { display: flex; flex-direction: row; border-radius: 10rpx; overflow: hidden; border: 1rpx solid #66bb6a; }
.segment-item { padding: 12rpx 24rpx; font-size: 26rpx; color: #66bb6a; background: #fff; }
.segment-item.active { background: #66bb6a; color: #fff; }

/* 主体区域 */
.body-wrap { 
  display: flex; 
  flex-direction: row; 
  flex: 1;
  overflow: hidden;
  margin: 0 24rpx;
  margin-bottom: 120rpx;
}

.sidebar {
  width: 160rpx;
  flex-shrink: 0;
  background: rgba(255,255,255,0.6);
  border-radius: 16rpx 0 0 16rpx;
  overflow: hidden;
}

.cat-item {
  padding: 28rpx 16rpx;
  font-size: 26rpx;
  color: #666666;
  text-align: center;
  border-left: 4rpx solid transparent;
  line-height: 1.4;
}

.cat-item.active {
  background: rgba(102, 187, 106, 0.2);
  color: #66bb6a;
  font-weight: 700;
  border-left-color: #66bb6a;
}

.product-area { 
  flex: 1; 
  padding: 16rpx;
  background: rgba(255,255,255,0.4);
  border-radius: 0 16rpx 16rpx 0;
}

.product-group { margin-bottom: 20rpx; }
.group-title {
  font-size: 32rpx; 
  font-weight: 700; 
  color: #333333;
  padding: 12rpx 0 10rpx;
  border-bottom: 2rpx solid rgba(102, 187, 106, 0.3);
  margin-bottom: 12rpx;
}

/* 商品网格布局 */
.product-grid {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  gap: 12rpx;
}

.product-card {
  width: calc(50% - 6rpx);
  padding: 12rpx;
  background: rgba(255, 255, 255, 0.9);
  border-radius: 16rpx;
  box-sizing: border-box;
  box-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.06);
}

.product-pic-wrap { 
  width: 100%; 
  height: 140rpx; 
  border-radius: 12rpx; 
  overflow: hidden; 
  background: rgba(255,255,255,0.8); 
}

.product-pic { width: 100%; height: 100%; display: block; }
.product-pic-placeholder {
  width: 100%; height: 100%;
  display: flex; align-items: center; justify-content: center;
  font-size: 32rpx; color: #999999;
}

.product-body { 
  display: flex; 
  flex-direction: column; 
  gap: 6rpx; 
  margin-top: 10rpx;
}

.product-title { 
  font-size: 26rpx; 
  font-weight: 600; 
  color: #333333; 
  line-height: 1.3;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
  min-height: 68rpx;
}

.product-desc { 
  font-size: 22rpx; 
  color: #999999; 
  line-height: 1.3; 
  display: -webkit-box;
  -webkit-line-clamp: 1;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.product-stock { font-size: 22rpx; color: #999999; }

.product-bottom { 
  display: flex; 
  flex-direction: row; 
  align-items: center; 
  justify-content: space-between; 
  margin-top: 6rpx; 
}

.product-price { font-size: 30rpx; font-weight: 700; color: #66bb6a; }
.product-actions { display: flex; align-items: center; }

.btn-select {
  padding: 0 20rpx; 
  height: 48rpx; 
  line-height: 48rpx;
  font-size: 24rpx; 
  color: #fff; 
  background: #66bb6a; 
  border-radius: 24rpx; 
  border: none;
  margin: 0;
}

.btn-select::after { border: none; }

.stepper { display: flex; align-items: center; gap: 10rpx; }
.stepper-btn { 
  width: 40rpx; 
  height: 40rpx; 
  text-align: center; 
  line-height: 40rpx; 
  font-size: 26rpx; 
  color: #666666; 
  background: rgba(0,0,0,0.06); 
  border-radius: 8rpx; 
}
.stepper-num { font-size: 24rpx; color: #333333; min-width: 32rpx; text-align: center; font-weight: 600; }

/* 底部占位 */
.bottom-placeholder {
  height: 40rpx;
}

/* 底部购物车栏 */
.cart-bar {
  position: fixed; 
  left: 0; 
  right: 0; 
  bottom: 0;
  padding: 16rpx 36rpx;
  padding-bottom: calc(16rpx + env(safe-area-inset-bottom));
  display: flex; 
  flex-direction: row; 
  align-items: center; 
  justify-content: space-between;
  background: rgba(255, 255, 255, 0.98);
  border-top: 1rpx solid rgba(0, 0, 0, 0.08);
  box-shadow: 0 -2rpx 12rpx rgba(0, 0, 0, 0.05);
  z-index: 100;
}

.cart-left { 
  display: flex; 
  flex-direction: row; 
  align-items: center; 
  gap: 16rpx; 
}

.cart-icon-wrap { 
  position: relative; 
  width: 60rpx;
  height: 60rpx;
  display: flex;
  align-items: center;
  justify-content: center;
}

.cart-icon { 
  font-size: 40rpx; 
}

.cart-badge {
  position: absolute; 
  top: -4rpx; 
  right: -4rpx;
  min-width: 32rpx; 
  height: 32rpx; 
  line-height: 32rpx; 
  text-align: center;
  font-size: 20rpx; 
  color: #fff; 
  background: #ff5252; 
  border-radius: 16rpx; 
  padding: 0 8rpx;
  font-weight: 600;
}

.cart-total-wrap { 
  display: flex; 
  flex-direction: column; 
  gap: 4rpx; 
}

.cart-total { 
  font-size: 36rpx; 
  font-weight: 700; 
  color: #333333; 
}

.cart-hint { 
  font-size: 22rpx; 
  color: #999999; 
}

.btn-checkout {
  padding: 0 48rpx; 
  height: 72rpx; 
  line-height: 72rpx;
  font-size: 30rpx; 
  color: #fff; 
  background: linear-gradient(90deg, #66bb6a, #43a047);
  border-radius: 36rpx; 
  border: none;
  margin: 0;
  font-weight: 600;
}

.btn-checkout::after { border: none; }
</style>
