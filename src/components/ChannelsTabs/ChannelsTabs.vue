<script setup lang="ts">
import { onBeforeUnmount, onMounted, ref } from 'vue'
import axios from 'axios'
import channelsList from './channels.json'

const emit = defineEmits<{
  (event: 'channelChange', selectedChannel: Channel): void
}>()

const updateTokenIntervalId = ref(0)

const channels = ref<Channel[]>(channelsList)

const currentChannel = ref<Channel>(channels.value[0])

const updateToken = async () => {
  try {
    const { data } = await axios.get(
      'https://mass.mako.co.il/ClicksStatistics/entitlementsServicesV2.jsp?et=ngt&lp=/direct/hls/live/2033791/k12dvr/index.m3u8?b-in-range=800-2700&rv=AKAMAI'
    )
    const token = data.tickets[0].ticket
    const originalUrl = channels.value[1].link
    const baseUrl = originalUrl.split('?')[0]

    channels.value[1].link = `${baseUrl}?${token}`
  } catch (error) {
    console.error('Error fetching token:', error)
  }
}

const emitChannelChange = (selectedChannel: Channel) => {
  emit('channelChange', selectedChannel)
}

onMounted(async () => {
  updateToken()
  updateTokenIntervalId.value = setInterval(updateToken, 5 * 60 * 1000)
})

onBeforeUnmount(() => {
  clearInterval(updateTokenIntervalId.value)
})
</script>

<template>
  <v-tabs v-model="currentChannel" align-tabs="center" center-active grow dir="rtl">
    <v-tab
      v-for="channel in channels"
      :key="channel.title"
      :value="channel"
      @click="emitChannelChange(channel)"
    >
      {{ channel.title }}
    </v-tab>
  </v-tabs>
</template>

<style scoped>
.v-tab {
  text-transform: none !important;
}
</style>
