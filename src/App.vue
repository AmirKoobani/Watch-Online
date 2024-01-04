<script setup lang="ts">
import { defineAsyncComponent, ref } from 'vue'
import channels from './components/ChannelsTabs/channels.json'

const ChannelsTabs = defineAsyncComponent(() =>
  import('./components').then(({ ChannelsTabs }) => ChannelsTabs)
)
const DarkModeToggle = defineAsyncComponent(() =>
  import('./components').then(({ DarkModeToggle }) => DarkModeToggle)
)
const VideoPlayer = defineAsyncComponent(() =>
  import('./components').then(({ VideoPlayer }) => VideoPlayer)
)

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
