<script setup lang="ts">
import { onBeforeUnmount, onMounted, ref } from 'vue'
import axios from 'axios'

const emit = defineEmits<{
  (event: 'channelChange', selectedChannel: Channel): void
}>()

const updateTokenIntervalId = ref(0)

const channels = ref<Channel[]>([
  {
    title: 'ערוץ 11',
    link: 'https://kan11w.media.kan.org.il/hls/live/2105694/2105694/source1_4k/chunklist.m3u8'
  },
  {
    title: 'ערוץ 12',
    link: 'https://mako-streaming.akamaized.net/direct/hls/live/2033791/k12dvr/index.m3u8'
  },
  {
    title: 'ערוץ 13',
    link: 'https://reshet.g-mana.live/media/87f59c77-03f6-4bad-a648-897e095e7360/profile/2/profileManifest.m3u8'
  },
  {
    title: 'ערוץ 14',
    link: 'https://now14.g-mana.live/media/91517161-44ab-4e46-af70-e9fe26117d2e/mainManifest.m3u8'
  },
  {
    title: 'Ynet',
    link: 'https://ynet-live-01.ynet-pic1.yit.co.il/ynet/live_720/index.m3u8'
  },
  {
    title: 'וואלה!',
    link: 'https://amg01742-walla-wallanews-ono-btlna.amagi.tv/playlist/amg01742-walla-wallanews-ono/playlist.m3u8'
  },
  {
    title: 'ערוץ 9',
    link: 'https://contactgbs.mmdlive.lldns.net/contactgbs/dd4f5f04932345f1b47c4bfb45fbd682/chunklist_b1128000.m3u8'
  },
  {
    title: 'ערוץ הכנסת',
    link: 'https://contact.gostreaming.tv/Knesset/myStream/chunklist_w882959991.m3u8'
  },
  {
    title: 'ערוץ הקניות',
    link: 'https://shoppingil-rewriter.vidnt.com/video_10801920_p_0.m3u8'
  }
])

const currentChannel = ref<Channel>(channels.value[0])

const updateToken = async () => {
  try {
    const { data } = await axios.get(
      'https://mass.mako.co.il/ClicksStatistics/entitlementsServicesV2.jsp?et=ngt&lp=/direct/hls/live/2033791/k12dvr/index.m3u8?b-in-range=800-2700&rv=AKAMAI'
    )
    const token = data.tickets[0].ticket
    channels.value[1].link = `https://mako-streaming.akamaized.net/direct/hls/live/2033791/k12dvr/index.m3u8?${token}`
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
