<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from 'vue'
import videojs from 'video.js'
import 'video.js/dist/video-js.css'

const { videoLink } = defineProps<{
  videoLink: string
}>()

const videoPlayer = ref<any>(null as any)

onMounted(() => {
  videoPlayer.value = videojs(videoPlayer.value, {
    autoplay: 'muted',
    controls: true,
    height: 800,
    // width: 600,
    sources: [
      {
        src: videoLink,
        type: 'application/x-mpegURL'
      }
    ]
  })
})

onBeforeUnmount(() => {
  if (videoPlayer.value) {
    videoPlayer.value.dispose()
  }
})
</script>

<template>
  <div>
    {{ videoLink }}
    <video ref="videoPlayer" class="video-js"></video>
  </div>
</template>

<style scoped>
/* Add your component-specific styles here */
</style>
