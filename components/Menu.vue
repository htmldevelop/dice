<template>
  <div class="menu">
    <div v-if="variant === 'desktop'" class="menu-desktop">
      <a
        v-for="(item, i) in desktop.items"
        :key="i"
        :class="{ 'active': page !== undefined && page === item.page }"
        class="menu-item"
        href="javascript:void(0);"
        @click="clickItem(item)"
      >
        {{ item.label }}
      </a>
    </div>
    <div v-if="variant === 'mobile'" class="menu-mobile">
      <div class="menu-mobile-items">
        <div
          v-for="(item, i) in mobile.items"
          :key="i"
          :class="{ 'active': page !== undefined && page === item.page }"
          class="menu-item"
          @click="clickItem(item)"
        >
          {{ item.label }}
        </div>
      </div>
      <div :class="mobile.statusOverlay" class="menu-mobile-options">
        <div v-if="getOverlayList(mobile.selectOverlay) && getOverlayList(mobile.selectOverlay).length > 0" class="menu-mobile-options-overflay">
          <a
            v-for="(item, i) in getOverlayList(mobile.selectOverlay)"
            :key="i"
            class="menu-mobile-options-overflay-item"
            href="javascript:void(0);"
          >
            {{ item.label }}
          </a>
        </div>
        <div
          v-for="(item, i) in mobile.overflay"
          :key="i"
          :class="[{ 'active': mobile.selectOverlay === item.label, 'before': mobile.beforeOverlay === item.label, 'disabled': item.active && item.active.indexOf(page) === -1 }, 'item-' + item.label]"
          class="menu-mobile-options-item"
          @click="clickOverlay(item)"
        />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    variant: {
      type: String,
      default: undefined
    }
  },
  data () {
    return {
      page: '',
      desktop: {
        items: [
          {
            label: 'Игры',
            page: ''
          },
          {
            label: 'Кран'
          },
          {
            label: 'Честность'
          },
          {
            label: 'Промокод'
          },
          {
            label: 'Поддержка'
          }
        ]
      },
      mobile: {
        statusOverlay: false,
        selectOverlay: false,
        beforeOverlay: false,
        activeOverlay: false,
        items: [
          {
            label: 'Игры',
            page: ''
          },
          {
            label: 'Кран'
          },
          {
            label: 'Конкурс'
          }
        ],
        overflay: [
          {
            label: 'options',
            active: [ 'coinflip' ],
            items: [
              {
                label: 'Проверить игру'
              },
              {
                label: 'Правила'
              },
              {
                label: 'Выкл. анимацию'
              },
              {
                label: 'Выкл. звук'
              }
            ]
          },
          {
            label: 'best'
          },
          {
            label: 'chat',
            to: 'chat'
          },
          {
            label: 'list',
            items: [
              {
                label: 'Честность'
              },
              {
                label: 'Промокод'
              }
            ]
          }
        ]
      }
    }
  },
  watch: {
    $route (route) {
      const page = (route.path).replace('/', '')
      this.$set(this, 'page', page)
      if (page === 'chat') {
        this.$set(this.mobile, 'statusOverlay', 'start')
        this.$set(this.mobile, 'selectOverlay', this.findItem(this.page).label)
        this.$set(this.mobile, 'activeOverlay', this.findItem(this.page).label)
      } else {
        this.$set(this.mobile, 'statusOverlay', 'hide')
        this.$set(this.mobile, 'beforeOverlay', this.mobile.selectOverlay)
        setTimeout(() => {
          this.$set(this.mobile, 'selectOverlay', false)
          this.$set(this.mobile, 'activeOverlay', false)
        }, 100)
      }
    }
  },
  methods: {

    clickOverlay (item) {
      if (item.to) {
        if (this.page === item.to) {
          this.$nuxt.$router.replace({ path: '/' })
        } else {
          this.$nuxt.$router.replace({ path: '/' + item.to })
        }
      } else if (!item.to) {
        if (this.mobile.selectOverlay === item.label) {
          this.$set(this.mobile, 'statusOverlay', 'hide')
          this.$set(this.mobile, 'beforeOverlay', this.mobile.selectOverlay)
          setTimeout(() => {
            this.$set(this.mobile, 'selectOverlay', false)
            this.$set(this.mobile, 'activeOverlay', false)
          }, 100)
        } else if (this.mobile.selectOverlay !== item.label) {
          if (this.mobile.statusOverlay === 'start') {
            this.$set(this.mobile, 'statusOverlay', 'hide')
            this.$set(this.mobile, 'beforeOverlay', this.mobile.selectOverlay)
            this.$set(this.mobile, 'activeOverlay', item.label)
            setTimeout(() => {
              this.$set(this.mobile, 'statusOverlay', 'start')
              this.$set(this.mobile, 'selectOverlay', item.label)
            }, 200)
          } else {
            this.$set(this.mobile, 'statusOverlay', 'start')
            this.$set(this.mobile, 'selectOverlay', item.label)
            this.$set(this.mobile, 'activeOverlay', item.label)
          }
        }
      }
    },

    clickItem (item) {
      if (item.page !== undefined) {
        this.$nuxt.$router.replace({ path: '/' + item.page })
      }
    },

    getOverlayList (label) {
      let items = []
      this.mobile.overflay.forEach((x, i) => {
        if (x.label === label) {
          items = x.items
        }
      })
      return items
    },

    findItem (label) {
      let item
      this.mobile.overflay.forEach((x, i) => {
        if (x.label === label) {
          item = x
        }
      })
      return item
    }

  }
}
</script>

<style>

.menu {
  flex: none;
}

.menu-mobile {
  height: 50px;
  display: none;
  align-items: center;
  justify-content: space-between;
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 0 10px;
  background: #381960;
  z-index: 3;
  border-top: 1px solid rgba(255, 255, 255, .1);
}

  @media (max-width: 767px) {
    .menu-mobile {
      display: flex;
    }
  }

.menu-mobile .menu-mobile-items {
  display: flex;
  align-items: flex-start;
  justify-content: flex-start;
  height: 100%;
}

.menu-mobile .menu-item {
  display: flex;
  align-items: center;
  justify-content: flex-start;
  flex-direction: column;
  font-size: 10px;
  color: #a698af;
  margin-right: 20px;
}

.menu-mobile .menu-item::before {
  content: '';
  background-image: url("~assets/images/icons/diamond.svg");
  width: 18px;
  height: 18px;
  background-position: center;
  background-repeat: no-repeat;
  background-size: contain;
  margin: 10px 0 2px;
}

.menu-mobile .menu-mobile-options {
  display: flex;
  align-items: center;
  justify-content: flex-start;
  border-radius: 5px;
  background-color: rgba(255, 255, 255, 0.1);
  padding: 8px 9px 8px 11px;
}

.menu-mobile .menu-mobile-options-item {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.menu-mobile .menu-mobile-options-item::before {
  content: '';
  background-position: center;
  background-repeat: no-repeat;
  background-size: contain;
}

.menu-mobile .menu-mobile-options-item.item-options::before {
  background-image: url("~assets/images/icons/icon-menu-options.svg");
  width: 15px;
  height: 15px;
  margin-right: 13px;
}

.menu-mobile .menu-mobile-options-item.item-best::before {
  background-image: url("~assets/images/icons/icon-menu-chips.svg");
  width: 18px;
  height: 17px;
  margin-right: 12px;
}

.menu-mobile .menu-mobile-options-item.item-chat::before {
  background-image: url("~assets/images/icons/icon-menu-support.svg");
  width: 18px;
  height: 18px;
  margin-right: 11px;
}

.menu-mobile .menu-mobile-options-item.item-list::before {
  background-image: url("~assets/images/icons/icon-menu-list.svg");
  width: 18px;
  height: 18px;
}

.menu-mobile .start .menu-mobile-options-item.active::before,
.menu-mobile .hide .menu-mobile-options-item.active::before {
  animation: showIconClose 0s ease-in-out forwards .1s;
}

.menu-mobile .start .menu-mobile-options-item.active,
.menu-mobile .hide .menu-mobile-options-item.active {
  animation: hideIcon .1s ease-in-out forwards, showIcon .1s ease-in-out forwards .1s;
}

.menu-mobile .hide .menu-mobile-options-item.before {
  animation: hideqIcon .1s ease-in-out forwards, showqIcon .1s ease-in-out forwards .1s;
}

.menu-mobile .hide .menu-mobile-options-item.item-options.before::before {
  animation: showIconOptions 0s ease-in-out forwards .1s;
  background-image: url("~assets/images/icons/icon-menu-close.svg");
}

.menu-mobile .hide .menu-mobile-options-item.item-best.before::before {
  animation: showIconBest 0s ease-in-out forwards .1s;
  background-image: url("~assets/images/icons/icon-menu-close.svg");
}

.menu-mobile .hide .menu-mobile-options-item.item-chat.before::before {
  animation: showIconChat 0s ease-in-out forwards .1s;
  background-image: url("~assets/images/icons/icon-menu-close.svg");
}

.menu-mobile .hide .menu-mobile-options-item.item-list.before::before {
  animation: showIconList 0s ease-in-out forwards .1s;
  background-image: url("~assets/images/icons/icon-menu-close.svg");
}

.menu-mobile .menu-mobile-options-item.disabled {
  opacity: .3;
  pointer-events: none;
}

.menu-mobile .menu-mobile-options-overflay {
  position: absolute;
  right: 9px;
  bottom: 50px;
  width: 150px;
  border-radius: 7px;
  border: solid 1px rgba(255, 255, 255, .15);
  background-color: #2c144c;
  padding: 13px 19px 16px;
  z-index: 3;
  opacity: 0;
  pointer-events: none;
}

.menu-mobile .start .menu-mobile-options-overflay {
  animation: showMore .2s ease-in-out forwards .1s;
  pointer-events: auto;
}

.menu-mobile .hide .menu-mobile-options-overflay {
  animation: hideMore .2s ease-in-out forwards;
  pointer-events: none;
}

.menu-mobile .menu-mobile-options-overflay-item {
  display: block;
  font-size: 14px;
  font-weight: normal;
  font-stretch: normal;
  font-style: normal;
  line-height: 1.71;
  letter-spacing: normal;
  color: #a698af;
  margin-bottom: 10px;
  text-decoration: none;
}

.menu-mobile .menu-mobile-options-overflay-item:last-child {
  margin-bottom: 0;
}

.menu-desktop {
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  top: -3px;
}

  @media (max-width: 767px) {
    .menu-desktop {
      display: none;
    }
  }

.menu-desktop .menu-item {
  font-size: 16px;
  font-weight: normal;
  font-stretch: normal;
  font-style: normal;
  line-height: 1.5;
  letter-spacing: normal;
  color: #a698af;
  cursor: pointer;
  margin-right: 16%;
  text-decoration: none;
  transition: color .2s ease-in-out;
}

.menu-desktop .menu-item:hover {
  color: #fff;
}

.menu-item {
  position: relative;
  height: 100%;
}

.menu-item:last-child {
  margin-right: 0;
}

.active.menu-item::after {
  content: '';
  height: 2px;
  background-color: #ffed2a;
  position: absolute;
  top: 100%;
  left: 0;
  right: 0;
}

  @media (max-width: 767px) {
    .active.menu-item::after {
      height: 3px;
      top: auto;
      bottom: 0;
    }
  }

.active.menu-item {
  color: #fff;
}

@keyframes showMore {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
    transform: translateY(-10px);
  }
}

@keyframes hideMore {
  0% {
    opacity: 1;
    transform: translateY(-10px);
  }
  100% {
    opacity: 0;
    transform: translateY(0);
  }
}

@keyframes hideIcon {
  0% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}

@keyframes showIcon {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

@keyframes hideqIcon {
  0% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}

@keyframes showqIcon {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

@keyframes showIconClose {
  100% {
    background-image: url("~assets/images/icons/icon-menu-close.svg");
  }
}

@keyframes showIconOptions {
  100% {
    background-image: url("~assets/images/icons/icon-menu-options.svg");
  }
}

@keyframes showIconBest {
  100% {
    background-image: url("~assets/images/icons/icon-menu-chips.svg");
  }
}

@keyframes showIconChat {
  100% {
    background-image: url("~assets/images/icons/icon-menu-support.svg");
  }
}

@keyframes showIconList {
  100% {
    background-image: url("~assets/images/icons/icon-menu-list.svg");
  }
}

</style>
