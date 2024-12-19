<template>
  <div class="background">
    <video
      class="fullscreenVideo"
      id="bgVid"
      playsinline
      autoplay
      muted
      loop
    >
      <source src="/videos/Coffeeshop.mp4" type="video/mp4" />
      Your browser does not support the video tag.
    </video>
  </div>
</template>

<script>
export default {
  name: "Background",
};
</script>

<style scoped>
.background {
  height: 100vh;
  width: 100vw;
  position: fixed;
  top: 0;
  left: 0;
  overflow: hidden;
  margin: 0;
  padding: 0;
}

.background:before {
  content: "";
  position: absolute;
  width: 100%;
  height: 100%;
  z-index: 1;
  top: 0;
  left: 0;
  background: rgba(25, 29, 34, 0.65);
}

.fullscreenVideo {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 100vw;
  height: 100vh;
  object-fit: cover; /* 确保视频填满容器 */
  object-position: center; /* 居中显示视频 */
  z-index: 0;
}

/* 兼容性优化，防止极端长宽比时出现空白 */
@media screen and (min-aspect-ratio: 16/9) {
  .fullscreenVideo {
    height: auto;
    width: 100vw;
  }
}

@media screen and (max-aspect-ratio: 16/9) {
  .fullscreenVideo {
    width: auto;
    height: 100vh;
  }
}
</style>
