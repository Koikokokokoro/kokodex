<template>
  <div class="player-content">
    <p>🎵 正在播放: {{ currentTrack.name }} by {{ currentTrack.artist }}</p>
    <audio ref="audioPlayer" controls autoplay=true>
      <source :src="resolveTrackSrc(currentTrack.src)" type="audio/mpeg" />
      您的浏览器不支持音频播放。
    </audio>
    <!-- <div class="controls">
      <button @click="prevTrack">上一首</button>
      <button @click="nextTrack">下一首</button>
    </div> -->
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, watch } from 'vue';

export default defineComponent({
  name: 'MusicPlayer',
  setup() {
    // 音轨列表
    const tracks = ref([
      { name: 'uzumakinoharu', artist: 'sasakure.UK',src: '/audios/uzumakinoharu.mp3' },
      // { name: 'Track 2', src: 'http://music.163.com/song/media/outer/url?id=2635812056.mp3' },
    ]);

    // 当前音轨索引和音轨
    const currentTrackIndex = ref(0);
    const currentTrack = ref(tracks.value[currentTrackIndex.value]);

    // 音频播放器引用
    const audioPlayer = ref<HTMLAudioElement | null>(null);

    // 切换到上一首
    const prevTrack = () => {
      currentTrackIndex.value = (currentTrackIndex.value - 1 + tracks.value.length) % tracks.value.length;
      currentTrack.value = tracks.value[currentTrackIndex.value];
    };

    // 切换到下一首
    const nextTrack = () => {
      currentTrackIndex.value = (currentTrackIndex.value + 1) % tracks.value.length;
      currentTrack.value = tracks.value[currentTrackIndex.value];
    };

    // 动态解析音轨的 src，支持 URL 和本地路径
    const resolveTrackSrc = (src: string) => {
      // 判断是否是绝对 URL
      if (src.startsWith('http://') || src.startsWith('https://')) {
        return src;
      }
      // 将相对路径解析为完整的文件路径
      return new URL(src, import.meta.url).href;
    };

    // 监听当前音轨变化并重新加载音频
    watch(currentTrack, () => {
      if (audioPlayer.value) {
        audioPlayer.value.load(); // 重新加载音频
        audioPlayer.value.play(); // 自动播放
      }
    });

    return { currentTrack, prevTrack, nextTrack, resolveTrackSrc, audioPlayer };
  },
});
</script>

<style scoped>
.player-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  width: 100%;
}

audio {
  width: 100%;
  margin-top: 10px;
  margin-left: 10%;
  margin-right: 10%;
}

.controls {
  margin-top: 10px;
}

button {
  background: none;
  border: 1px solid white;
  color: white;
  padding: 5px 10px;
  margin: 0 5px;
  border-radius: 5px;
  cursor: pointer;
  transition: background 0.3s ease;
}

button:hover {
  background: white;
  color: black;
}
</style>