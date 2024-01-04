<script setup lang="ts">
import { onBeforeUnmount, onMounted, ref, watch } from 'vue'
import videojs from 'video.js'
import Player from 'video.js/dist/types/player'
import hebrew from './videoJSHeDictionary.json'
import 'video.js/dist/video-js.css'

videojs.log.level('off')
videojs.addLanguage('he', hebrew)

const props = defineProps<{
  videoLink: string
}>()

const videoPlayer = ref<Player | null>(null)

onMounted(() => {
  videoPlayer.value = videojs('videoPlayerId', {
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

watch(
  () => props.videoLink,
  (newVideoLink) => {
    const currentPlayer = videojs.getPlayer('videoPlayerId')

    currentPlayer.src({ src: newVideoLink, type: 'application/x-mpegURL' })
  }
)
</script>

<template>
  <video id="videoPlayerId" ref="videoPlayer" class="video-js"></video>
</template>

<style scoped></style>
