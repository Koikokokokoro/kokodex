<template>
  <Background class="bg" :class="{ blurred: isDrawerVisible || isPlayerVisible }"/>
  <div :class="['player', { 'player-visible': isPlayerVisible }]">
    <MusicPlayer />
  </div>
  <div class="app" :class="{ blurred: isDrawerVisible || isPlayerVisible }">
    <div :class="['overlay', { 'overlay-visible': isDrawerVisible || isPlayerVisible }]" />
    <div class="profile-container">
        <img class="avatar" src="./assets/head.jpg" alt="Avatar" />
        <p class="description">Hi! 欢迎来到Koikokokokoro的主页！</p>
    </div>
  </div>
  <!-- 抽屉 -->
  <div :class="['drawer', { 'drawer-visible': isDrawerVisible }]">
    <DrawerLinks />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted, onUnmounted } from 'vue';
import DrawerLinks from './components/DrawerLinks.vue';
import MusicPlayer from './components/MusicPlayer.vue';
import Background from './components/Background.vue';

export default defineComponent({
  components: { DrawerLinks, MusicPlayer, Background },
  setup() {
    const isDrawerVisible = ref(false);
    const isPlayerVisible = ref(false);

    const handleWheel = (event: WheelEvent) => {
      if (event.deltaY > 0) {
        if (isPlayerVisible.value) {
          // 如果 MusicPlayer 可见，隐藏它
          isPlayerVisible.value = false;
        } else if (!isDrawerVisible.value) {
          // 如果 MusicPlayer 已隐藏且 Drawer 未显示，则显示 Drawer
          isDrawerVisible.value = true;
        }
      } else if (event.deltaY < 0) {
        // 向上滚动
        if (isDrawerVisible.value) {
          // 如果 Drawer 可见，隐藏它
          isDrawerVisible.value = false;
        } else if (!isPlayerVisible.value) {
          // 如果 Drawer 已隐藏且 MusicPlayer 未显示，则显示 MusicPlayer
          isPlayerVisible.value = true;
        }
      }
    };

    onMounted(() => {
      window.addEventListener('wheel', handleWheel);
    });

    onUnmounted(() => {
      window.removeEventListener('wheel', handleWheel);
    });

    return { isDrawerVisible, isPlayerVisible };
  },
});
</script>

<style scoped>
::-webkit-scrollbar {
  display: none;
}

body {
  -ms-overflow-style: none;  /* Windows Edge, IE 浏览器的滚动条隐藏 */
  scrollbar-width: none;  /* Firefox 浏览器的滚动条隐藏 */
}

.bg {
  text-align: center;
  overflow: hidden;
  transition: filter 0.5s ease;
}
.bg.blurred {
  filter: blur(5px);
}
.app {
  height: 100vh;
  width: 100vw;
  /* background-size: cover; */
  background-position: center;
  text-align: center;
  overflow: hidden;
  position: relative;
  margin: 0;
  padding: 0;
  z-index: 0;
  /* backdrop-filter: blur(4px); */

  transition: filter 0.5s ease;
}

.app.blurred{
  filter: blur(5px);
}

/* 蒙版样式 */
.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0);
  z-index: 1;
  transition: background-color 0.5s ease;
}
.overlay-visible {
  background-color: rgba(0, 0, 0, 0.5);
}

/* 背景虚化 */
.profile-container {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  text-align: center;
  z-index: 3;
}

/* 头像样式 */
.avatar {
  width: 150px;
  height: 150px;
  border-radius: 50%;
  border: 2px solid white;
}
.description {
  margin-top: 10px;
  font-size: 1.2em;
  background-color: rgb(240, 248, 255, 0.5);
  border-radius: 5px;
}

/* 抽屉样式 */
.drawer {
  position: fixed;
  bottom: -100%; /* 默认隐藏 */
  left: 0;
  width: 90%;
  margin-left: 5%;
  height: 20%;
  background-color: rgba(0, 0, 0, 0.5);
  color: white;
  display: flex;
  justify-content: center;
  align-items: center;
  transition: bottom 0.5s ease; /* 平滑动画 */
  z-index: 3;
  border-radius: 10px;
}
.drawer-visible {
  bottom: 0; /* 抽屉弹出 */
}
.player {
  position: fixed;
  top: -100%; /* 默认隐藏 */
  left: 0;
  width: 90%;
  margin-left: 5%;
  height: 20%;
  background-color: rgba(0, 0, 0, 0.5);
  color: white;
  display: flex;
  justify-content: center;
  align-items: center;
  transition: top 0.5s ease; /* 平滑动画 */
  z-index: 4;
  border-radius: 10px;
}
.player-visible {
  top: 0; /* 抽屉弹出 */
}
</style>
