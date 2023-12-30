<script setup lang="ts">
import { onBeforeUnmount, onMounted, ref, watch } from 'vue'
import videojs from 'video.js'
import 'video.js/dist/video-js.css'

const props = defineProps<{
  videoLink: string
}>()

const videoPlayer = ref<any>(null as any)

onMounted(() => {
  videoPlayer.value = videojs("videoPlayerId", {
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
  if (videoPlayer.value) {
    videoPlayer.value.dispose()
  }
})

watch(() => props.videoLink, (newVideoLink) => {
  const currentPlayer = videojs.getPlayer("videoPlayerId")

  currentPlayer.src({ src: newVideoLink, type: 'application/x-mpegURL' })
})
</script>

<template>
  <video id="videoPlayerId" ref="videoPlayer" class="video-js"></video>
</template>

<style scoped>
/* Add your component-specific styles here */
</style>
