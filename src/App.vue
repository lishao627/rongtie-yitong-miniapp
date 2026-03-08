<script>
export default {
  onLaunch: function () {
    console.log('App Launch')
  },
  onShow: function () {
    console.log('App Show')
  },
  onHide: function () {
    console.log('App Hide')
  },
}
</script>

<style>
/*每个页面公共css */

/* 修改 TabBar 字体大小 */
.uni-tabbar .uni-tabbar__label {
  font-size: 16px !important;
  font-weight: 500;
}

/* 选中状态的字体加粗 */
.uni-tabbar .uni-tabbar__item--active .uni-tabbar__label {
  font-weight: 700;
}
</style>
