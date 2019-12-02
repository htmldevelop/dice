<template>
  <div :class="'wrapper--' + page" class="wrapper">
    <header v-if="page !== 'chat'" class="header">
      <div class="header-wrap">
        <nuxt-link to="/" class="header-logo" />
        <Menu variant="desktop" />
        <div class="header-options">
          <div class="header-options-money">
            305.69₽
          </div>
          <a href="javascript:void(0);" class="header-options-wallet">
            Кошелек
          </a>
          <a href="javascript:void(0);" class="header-options-profile" />
        </div>
      </div>
    </header>
    <div class="content">
      <nuxt />
    </div>
    <footer class="footer">
      <Menu variant="mobile" />
    </footer>
  </div>
</template>

<script>
import Menu from '~/components/Menu.vue'

export default {
  head: {
    meta: [
      { name: 'viewport', content: 'width=device-width,maximum-scale=1,initial-scale=1,user-scalable=no' },
      { name: 'format-detection', content: 'telephone=no' },
      { name: 'apple-mobile-web-app-capable', content: 'yes' }
    ]
  },
  components: {
    Menu
  },
  data () {
    return {
      page: ''
    }
  },
  watch: {
    $route (route) {
      this.$set(this, 'page', (route.path).replace('/', ''))
    }
  }
}
</script>

<style>
html,
body {
  height: 100%;
  width: 100%;
  overflow-x: hidden;
}

html {
  font-family: 'Source Sans Pro', sans-serif;
  font-size: 16px;
  word-spacing: 1px;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
  box-sizing: border-box;
}

*,
*:before,
*:after {
  font-family: 'Source Sans Pro', sans-serif;
  font-size: 16px;
  box-sizing: border-box;
  margin: 0;
  outline: none;
  color: #fff;
  -webkit-tap-highlight-color: transparent;
  -webkit-appearance: none;
  transition: none;
}

body {
  background: #381960;
  -webkit-text-size-adjust: none;
  position: relative;
}

ul {
  display: block;
  list-style-type: none;
  padding: 0;
  margin: 0;
}

#__nuxt,
#__layout {
  height: 100%;
}

.wrapper {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-direction: column;
  background: #381960;
  min-height: 100%;
  width: 100%;
  overflow: hidden;
}

  @media all and (max-width: 767px) {
    .wrapper {
      justify-content: flex-start;
    }
  }

.wrapper.wrapper-- {
  background: #291a43;
}

.wrapper.wrapper-- .content-wrap {
  display: flex;
  align-items: flex-start;
  justify-content: center;
  padding: 30px;
  background: #291a43;
}

  @media all and (max-width: 767px) {
    .wrapper.wrapper-- .content-wrap {
      padding: 0;
    }
  }

.header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  flex: none;
  width: 100%;
  padding-top: 24px;
  background: #381960;
  z-index: 3;
  height: 82px;
}

  @media (max-width: 767px) {
    .header {
      height: 60px;
      padding-top: 13px;
    }
  }

.header-logo {
  flex: none;
  width: 110px;
  height: 29px;
  background: url('~assets/images/logo.svg') no-repeat left center;
  background-size: contain;
  text-decoration: none;
}

  @media (max-width: 767px) {
    .header-logo {
      width: 33px;
      height: 30px;
      background-image: url('~assets/images/logo--mini.svg');
    }
  }

.content {
  flex: 1 1;
  width: 100%;
  position: relative;
  padding: 82px 0 0;
  z-index: 1;
  overflow: scroll;
}

  @media (max-width: 767px) {
    .content {
      padding: 60px 0 50px;
    }
    .content--inside {
      background: #291a43;
    }
  }

.footer {
  flex: none;
  width: 100%;
}

.header-wrap,
.content-wrap {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 2.5%;
}

  @media (max-width: 767px) {
    .header-wrap,
    .content-wrap {
      padding: 0 10px;
    }
  }

.header-wrap {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.header-options {
  display: flex;
  align-items: center;
  justify-content: flex-start;
  flex: none;
}

.header-options-money {
  font-size: 26px;
  font-weight: bold;
  font-stretch: normal;
  font-style: normal;
  line-height: 1.23;
  letter-spacing: normal;
  text-align: right;
  color: #ffed2a;
}

  @media all and (max-width: 767px) {
    .header-options-money {
      font-size: 20px;
    }
  }

.header-options-wallet {
  display: flex;
  align-items: center;
  width: 86px;
  height: 34px;
  font-size: 12px;
  font-weight: bold;
  font-stretch: normal;
  font-style: normal;
  line-height: 1.5;
  letter-spacing: normal;
  color: #fff;
  text-transform: uppercase;
  margin-left: 15px;
  background: url('~assets/images/icons/icon-wallet.png') no-repeat center;
  background-size: contain;
  padding-left: 11px;
  transition: background .2s ease-in-out;
  cursor: pointer;
  text-decoration: none;
}

.header-options-wallet:hover {
  background-image: url('~assets/images/icons/icon-wallet--hover.png');
}

  @media all and (max-width: 767px) {
    .header-options-wallet {
      margin-left: 11px;
    }
  }

  @media all and (min-width: 1200px) {
    .header-options-wallet:active {
      background-image: url('~assets/images/icons/icon-wallet--active.png');
    }
  }

.header-options-profile {
  width: 26px;
  height: 26px;
  margin-left: 15px;
  background: url('~assets/images/icons/icon-profile.svg') no-repeat center;
  background-size: contain;
  transition: background .2s ease-in-out;
  cursor: pointer;
  text-decoration: none;
}

.header-options-profile:hover {
  background-image: url('~assets/images/icons/icon-profile--hover.svg');
}

  @media all and (max-width: 767px) {
    .header-options-profile {
      margin-left: 11px;
    }
  }

  @media all and (min-width: 1200px) {
    .header-options-profile:active {
      background-image: url('~assets/images/icons/icon-profile--active.svg');
    }
  }
</style>
