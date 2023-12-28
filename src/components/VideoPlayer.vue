<script setup lang="ts">
import { ref, defineProps, onMounted, onBeforeUnmount, watch } from 'vue'
import videojs from 'video.js'
import 'video.js/dist/video-js.css'

type MyComponentProps = {
    options: Record<string, any>
}

const { options } = defineProps<MyComponentProps>()

const videoOptions = ref(options)
const videoPlayer = ref<any>(null as any)

onMounted(() => {
    videoPlayer.value = videojs(videoPlayer.value, videoOptions.value)
})

onBeforeUnmount(() => {
    if (videoPlayer.value) {
        videoPlayer.value.dispose()
    }
})

watch(() => options, (newOptions) => {
    videoOptions.value = newOptions
    // Now manually update the video player when options change
    if (videoPlayer.value) {
        videoPlayer.value.src(videoOptions.value.sources)
    }
})
</script>

<template>
    <div>
        <video ref="videoPlayer" class="video-js"></video>
    </div>
</template>

<style scoped>
/* Add your component-specific styles here */
</style>
