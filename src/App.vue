<template>
  <div class="top">
    <!-- navigation -->
    <nav>
      <router-link to='/main/sideBar' tag='span' class='menu'></router-link>
      <span :class='{focus: currentSlider === 0}' @click='switchSlider($event, 0)'>我的</span>
      <span :class='{focus: currentSlider === 1}' @click='switchSlider($event, 1)'>发现</span>
      <span :class='{focus: currentSlider === 2}' @click='switchSlider($event, 2)'>动态</span>
      <router-link to='/search' tag='span' class='search'></router-link>
    </nav>
    <!-- three components -->
    <section class='threeSlide'>
      <mine-music-component :style="{'margin-left': (-100 * currentSlider) + '%'}"></mine-music-component>
      <discovery-component></discovery-component>
      <activity-component></activity-component>
    </section>
    <transition :name="transitionName">
      <keep-alive>
        <router-view></router-view>
      </keep-alive>
    </transition>
  </div>
</template>

<script>
import mineMusicComponent from './components/mineMusic.vue'
import discoveryComponent from './components/discovery.vue'
import activityComponent from './components/activity.vue'

import hideLoader from './tools/hideLoader'
export default {
  name: 'mainUI',
  data () {
    return {
      currentSlider: 0,
      transitionName: ''
    }
  },
  components: {
    mineMusicComponent,
    discoveryComponent,
    activityComponent
  },
  mounted () {
    hideLoader()
  },
  methods: {
    switchSlider (e, num) {
      this.currentSlider = num
    }
  },
  // 定义主路由过渡动效
  watch: {
    '$route' (to, from) {
      // console.log(to, from)
      if (to.name === 'songList' || from.name === 'songList') {
        this.transitionName = 'fadeInUp'
      } else if (to.name === 'sideBar' || from.name === 'sideBar') {
        this.transitionName = 'slideInRight'
      } else {
        this.transitionName = ''
      }
    }
  }
}
</script>

<style lang='less' scoped>
  div.top {
    width: 100%;
    height: 100%;
    position: relative;
  }
  nav {
    height: 3em;
    background-color: red;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    color:#ddd;
    padding: 0 1em;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    > span {
      font-family: 'icomoon' !important;
    }
    > span.focus {
        color: #fff;
    }
    > span:first-child {
      &::before {
        font-size: 1.2em;
        content: "\e9bb"
      }
    }
    > span:last-child {
      &::before {
        font-size: 1.2em;
        content: "\e986"
      }
    }
  }
  section.threeSlide {
    width: 100%;
    display: flex;
    flex-direction: row;
    overflow: hidden;
    > div {
      width: 100%;
      height: 100%;
      flex-shrink: 0;
      transition: all .3s ease;
    }
  }
  /*router-view transition styles*/
  .fadeInUp-enter-active, .fadeInUp-leave-active, .fadeOutDown-leave-active, .fadeOutDown-enter-active {
    transition: all .3s ease;
  }
  .slideInRight-enter-active, .slideInRight-leave-avtive {
    transition: all .3s ease;
  }

  .fadeInUp-enter{
    opacity: 0;
    transform: translate3d(0, 50%, 0);
  }

  .fadeInUp-leave-to {
    opacity: 0;
    transform: translate3d(0, 50%, 0);
  }

  .slideInRight-enter {
    opacity: 0;
    transform: translate3d(-100%, 0, 0);
  }

  .slideInRight-leave-to {
    opacity: 0;
    transform: translate3d(-100%, 0, 0);
  }
</style>
<style>
@font-face {
  font-family: 'icomoon';
  src:  url('./assets/icomoon.eot');
  src:  url('./assets/icomoon.eot') format('embedded-opentype'),
    url('./assets/icomoon.ttf') format('truetype'),
    url('./assets/icomoon.woff') format('woff'),
    url('./assets/icomoon.svg') format('svg');
  font-style: normal;
}
</style>
