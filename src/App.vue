<script setup lang="ts">
import { ref } from 'vue'
import { ChannelsTabs, DarkModeToggle, VideoPlayer } from './components'
import channels from './components/ChannelsTabs/channels.json'

const currentChannel = ref<Channel>(channels.slice(-1)[0])

const handleChannelChange = (selectedChannel: Channel) => {
  currentChannel.value = selectedChannel
}
</script>

<template>
  <v-app>
    <v-app-bar app>
      <ChannelsTabs
        :channels="channels"
        :current-channel="currentChannel"
        @channel-change="handleChannelChange"
      />
    </v-app-bar>
    <v-main>
      <DarkModeToggle />
      <v-container>
        <VideoPlayer :video-link="currentChannel.link" />
      </v-container>
    </v-main>
  </v-app>
</template>

<style scoped></style>
