<script>
import { reactive, computed, watchEffect, onMounted, onBeforeUnmount } from 'vue'
import axios from 'axios'
import { useStore } from 'vuex'
import { useRouter, useRoute } from 'vue-router'

export default {
  name: 'Video',
  setup() {
    const store = useStore()
    const router = useRouter()

    // 添加固定的视频源
    const fixedVideos = [
      {
        id: 'fixed1',
        url: 'https://1333405442.vod-qcloud.com/6e879a0evodtranssh1333405442/74af45a41397757900005715355/v.f100010.mp4'
      },
      {
        id: 'fixed2',
        url: 'https://1333405442.vod-qcloud.com/6e879a0evodtranssh1333405442/a89a65881397757900009719225/v.f100010.mp4'
      },
      {
        id: 'fixed3',
        url: 'https://1333405442.vod-qcloud.com/6e879a0evodtranssh1333405442/5c1b8f0d1397757900008783383/v.f100010.mp4'
      },
      {
        id: 'fixed4',
        url: 'https://1333405442.vod-qcloud.com/0f942347vodtranscq1333405442/21b6b6001397757900011261727/v.f100010.mp4'
      }
    ]

    let data = reactive({
      cameras: store.state.camera,
      videoInstances: [], // 存储 videojs 实例
      fixedVideos: fixedVideos // 添加固定视频数组
    })

    function initVideoSourc(a) {
      const player = videojs(a, {
        bigPlayButton: false,
        textTrackDisplay: false,
        posterImage: false,
        errorDisplay: false,
        controlBar: true,
      }, function () {
        this.play()
      })
      data.videoInstances.push(player)
    }

    onMounted(() => {
      // 初始化固定视频
      fixedVideos.forEach((video, index) => {
        initVideoSourc("video-" + video.id)
      })
    })

    onBeforeUnmount(() => {
      data.videoInstances.forEach(player => {
        if (player && typeof player.dispose === 'function') {
          player.dispose()
        }
      })
      data.videoInstances = []

      const videos = document.querySelectorAll('.video-item')
      videos.forEach(video => {
        video.pause()
        video.removeAttribute('src')
        video.load()
      })

      if (window.videojs) {
        window.videojs.getAllPlayers().forEach(player => {
          if (player && typeof player.dispose === 'function') {
            player.dispose()
          }
        })
      }
    })

    return {
      data,
    }
  },
}
</script>

<template>
  <div class="video-container">
    <video v-for="video in data.fixedVideos"
           :key="video.id"
           :id="'video-' + video.id"
           class="video-item"
           autoplay
           muted
           loop
           playsinline>
      <source :src="video.url" type="video/mp4">
    </video>
  </div>
</template>


<style scoped>
.video-container {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: 1fr 1fr;
  gap: 20px;
  width: 100%;
  height: 100%;
}

.video-item {
  width: 100%;
  height: 100%;
}
</style>
