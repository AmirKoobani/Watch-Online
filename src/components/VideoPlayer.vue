<script setup lang="ts">
import { onBeforeUnmount, onMounted, ref, watch } from 'vue'
import videojs from 'video.js'
import 'video.js/dist/video-js.css'

const props = defineProps<{
  videoLink: string
}>()

const player = ref<any>(null as any)
const videoPlayer = ref<any>(null as any)

onMounted(() => {
  player.value = videojs(videoPlayer.value, {
    autoplay: 'muted',
    controls: true,
    fluid: true,
    sources: [
      {
        src: props.videoLink,
        type: 'application/x-mpegURL'
      }
    ]
  })
})

onBeforeUnmount(() => {
  if (player.value) {
    player.value.dispose()
  }
})

watch(() => props.videoLink, (newVideoLink) => {
  const currentPlayer = videojs.getPlayer("vjs_video_3")
  currentPlayer.src({ src: newVideoLink, type: 'application/x-mpegURL' })
})
</script>

<template>
  <video ref="videoPlayer" class="video-js"></video>
</template>

<style scoped>
/* Add your component-specific styles here */
</style>
