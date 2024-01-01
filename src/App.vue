<script setup lang="ts">
import { ref } from 'vue'
import { ChannelsTabs, DarkModeToggle, VideoPlayer } from './components';
import channelsList from './components/ChannelsTabs/channels.json';

const currentChannel = ref<Channel>(channelsList[0])

const handleChannelChange = (selectedChannel: Channel) => {
  currentChannel.value = selectedChannel
}
</script>

<template>
  <v-app>
    <v-app-bar app>
      <ChannelsTabs @channel-change="handleChannelChange" />
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
