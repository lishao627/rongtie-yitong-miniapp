<template>
  <!-- 我的收藏页面：延续小程序设计风格 -->
  <view class="page">
    <!-- 顶部导航栏 -->
    <view class="header">
      <view class="header-left" @tap="goBack">
        <text class="header-icon">←</text>
      </view>
      <text class="header-title">我的收藏</text>
      <view class="header-right" @tap="editFavorites">
        <text class="header-action">{{ isEditing ? '完成' : '编辑' }}</text>
      </view>
    </view>

    <!-- 收藏列表 -->
    <view class="content-wrap">
      <view class="product-grid">
        <view 
          v-for="(product, index) in favorites" 
          :key="product.id"
          class="product-card"
          @tap="goProductDetail(product.id)"
        >
          <view class="product-image">
            <image :src="product.image" mode="aspectFill" />
            <view v-if="isEditing" class="delete-badge" @tap.stop="removeFavorite(product.id)">
              <text class="delete-icon">×</text>
            </view>
          </view>
          <view class="product-info">
            <text class="product-name">{{ product.name }}</text>
            <text class="product-price">¥{{ product.price }}</text>
          </view>
        </view>
      </view>

      <!-- 空收藏提示 -->
      <view v-if="favorites.length === 0" class="empty-favorites">
        <text class="empty-icon">收藏</text>
        <text class="empty-text">暂无收藏商品</text>
        <text class="empty-hint">收藏喜欢的商品，方便下次购买</text>
        <button class="btn-go-shop" type="default" @tap="goShop">去逛逛</button>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      isEditing: false,
      favorites: [
        {
          id: '1',
          name: '苹果可溶性商标',
          price: '19.9',
          image: '/static/apple.jpg'
        },
        {
          id: '2',
          name: '梨可溶性商标',
          price: '21.9',
          image: '/static/梨子.jpg'
        },
        {
          id: '3',
          name: '草莓可溶性商标',
          price: '28.9',
          image: '/static/草莓.jpg'
        },
        {
          id: '4',
          name: '葡萄可溶性商标',
          price: '26.9',
          image: '/static/grape.jpg'
        }
      ]
    }
  },
  methods: {
    goBack() {
      uni.navigateBack()
    },
    editFavorites() {
      this.isEditing = !this.isEditing
    },
    goProductDetail(id) {
      if (!this.isEditing) {
        uni.navigateTo({ url: '/pages/product/detail?id=' + id })
      }
    },
    removeFavorite(id) {
      uni.showModal({
        title: '提示',
        content: '确定要取消收藏该商品吗？',
        success: (res) => {
          if (res.confirm) {
            this.favorites = this.favorites.filter(item => item.id !== id)
            uni.showToast({ title: '已取消收藏', icon: 'success' })
          }
        }
      })
    },
    goShop() {
      uni.switchTab({ url: '/pages/index/index' })
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
}

/* 收藏列表 */
.content-wrap {
  padding: 24rpx 36rpx;
}

.product-grid {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  gap: 20rpx;
}

.product-card {
  width: calc(50% - 10rpx);
  background: rgba(255, 255, 255, 0.95);
  border-radius: 20rpx;
  box-shadow: 0 2rpx 12rpx rgba(0, 0, 0, 0.08);
  overflow: hidden;
}

.product-image {
  width: 100%;
  height: 240rpx;
  position: relative;
  overflow: hidden;
}

.product-image image {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.delete-badge {
  position: absolute;
  top: 16rpx;
  right: 16rpx;
  width: 48rpx;
  height: 48rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(255, 0, 0, 0.8);
  border-radius: 50%;
}

.delete-icon {
  font-size: 32rpx;
  color: #ffffff;
  font-weight: bold;
}

.product-info {
  padding: 20rpx;
  display: flex;
  flex-direction: column;
  gap: 12rpx;
}

.product-name {
  font-size: 28rpx;
  color: #333333;
  font-weight: 600;
  line-height: 1.3;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
  min-height: 72rpx;
}

.product-price {
  font-size: 32rpx;
  color: #66bb6a;
  font-weight: 700;
}

/* 空收藏提示 */
.empty-favorites {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 120rpx 36rpx;
}

.empty-icon {
  font-size: 80rpx;
  color: #cccccc;
  margin-bottom: 24rpx;
}

.empty-text {
  font-size: 32rpx;
  color: #666666;
  margin-bottom: 16rpx;
}

.empty-hint {
  font-size: 26rpx;
  color: #999999;
  margin-bottom: 40rpx;
}

.btn-go-shop {
  padding: 0 56rpx;
  height: 72rpx;
  line-height: 72rpx;
  font-size: 30rpx;
  color: #ffffff;
  background: linear-gradient(90deg, #66bb6a, #43a047);
  border-radius: 36rpx;
  border: none;
}

.btn-go-shop::after {
  border: none;
}
</style>