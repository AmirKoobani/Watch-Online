<script setup lang="ts">
import { ref } from 'vue'
import ChannelsTabs from './components/ChannelsTabs.vue'
import VideoPlayer from './components/VideoPlayer/VideoPlayer.vue'

const currentChannel = ref<Channel>({
  title: 'ערוץ 11',
  link: 'https://kan11w.media.kan.org.il/hls/live/2105694/2105694/source1_4k/chunklist.m3u8'
})

const handleChannelChange = (selectedChannel: Channel) => {
  currentChannel.value = selectedChannel
}
</script>

<template>
  <v-app>
    <v-app-bar color="blue" app>
      <ChannelsTabs @channel-change="handleChannelChange" />
    </v-app-bar>
    <v-main>
      <v-container>
        <VideoPlayer :video-link="currentChannel.link" />
      </v-container>
    </v-main>
  </v-app>
</template>

<style scoped></style>
