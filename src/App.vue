<template>
  <div id="app">
    <el-backtop></el-backtop>
    <router-view v-if="isRouterAlive" />
    <Footer></Footer>
  </div>
</template>

<script>
import Footer from './components/common/Footer'
export default {
  name: 'app',
  provide () {
    return {
      reload: this.reload,
      sideScroll: this.sideScroll
    }
  },
  data () {
    return {
      isRouterAlive: true
    }
  },
  components: {
    Footer
  },
  methods: {
    reload () {
      this.isRouterAlive = false
      this.$nextTick(() => {
        this.isRouterAlive = true
      })
    },
    sideScroll () {
      if (this.$route.name === 'home' || this.$route.name === 'article') {
        if (document.documentElement.clientWidth < 768) {
          return
        }
        const sideBar = document.getElementById('sideBar') // 子盒子
        if (!sideBar) {
          return
        }
        const sideBarWrap = document.getElementById('sideBarWrap') // 父盒子
        const sideBarWrapHeigth = sideBarWrap.offsetHeight // 父盒子高度
        const sidBarHeigth = sideBar.offsetHeight // 子盒子的高度
        // if (sideBarWrapHeigth < sidBarHeigth) {
        //   sideBarWrap.style.height = sidBarHeigth + 'px'
        // }
        const sideBarWrapOffsetTop = sideBarWrap.offsetTop // 盒子距顶部
        const scrollTop = document.body.scrollTop + document.documentElement.scrollTop // 卷去高度
        const clientHeight = document.documentElement.clientHeight // 可视区高度
        // 距离顶部的高度 + 自身的高度 - 可视区的高度 = 需要被卷去的距离
        const moveY = sideBarWrapOffsetTop + sidBarHeigth - clientHeight // 需要被卷去的距离
        const footer = document.getElementById('footer')
        const footerClientHeight = footer.offsetTop - scrollTop // footer距顶部
        if (scrollTop > moveY) {
          if (footerClientHeight > clientHeight) {
            sideBar.style.marginTop = scrollTop - moveY + 'px' // 记得加单位
          } else {
            // 否则定位到最底部
            if (sideBarWrapHeigth > sidBarHeigth) {
              sideBar.style.marginTop = sideBarWrap.offsetHeight - sidBarHeigth + 'px' // 记得加单位
            }
          }
        } else {
          sideBar.style.marginTop = 0
        }
      }
    }
  },
  mounted () {
    this.sideScroll()
    // this.$nextTick(() => {
    //   this.sideScroll()
    // })
    window.addEventListener('scroll', this.sideScroll)
  },
  watch: {
    $route (to, from) {
      setTimeout(() => {
        const sideBar = document.getElementById('sideBar')
        if (sideBar) {
          sideBar.style.marginTop = 0
        }
      }, 300)
    }
  }
}
</script>

<style lang="less" scoped>
#app {
  // height: 100%;
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  font-family: font-regular, 'Helvetica Neue', sans-serif;
  font-size: 16px;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #304455;
  background-color: #edf0f2;
  // position: relative;
}
</style>
