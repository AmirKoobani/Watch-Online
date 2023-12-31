<script setup lang="ts">
import ChannelsTabs from './components/ChannelsTabs.vue'
import VideoPlayer from './components/VideoPlayer/VideoPlayer.vue'

const handleSelectedChannel = (selectedChannelTab: ChannelTab) => {
  // Update the currentChannelTab in the parent component
  currentChannelTab.value = selectedChannelTab
}
</script>

<template>
  <v-app>
    <v-app-bar color="blue" app>
      <ChannelsTabs @selected-channel="handleSelectedChannel" />
    </v-app-bar>
    <v-main>
      <v-container>
        <VideoPlayer :video-link="currentChannelTab.link" />
      </v-container>
    </v-main>
  </v-app>
</template>

<style scoped></style>
