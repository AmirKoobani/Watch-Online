<script setup lang="ts">
import { onBeforeUnmount, onMounted, ref } from 'vue'
import axios from 'axios'

const updateTicketId = ref(0)

const channelsTabs = ref<ChannelTab[]>([
  {
    title: 'ערוץ 11',
    link: 'https://kan11w.media.kan.org.il/hls/live/2105694/2105694/source1_4k/chunklist.m3u8'
  },
  {
    title: 'ערוץ 12',
    link: ''
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

const currentChannelTab = ref<ChannelTab>(channelsTabs.value[0])

const updateTicket = async () => {
  try {
    const { data } = await axios.get(
      'https://mass.mako.co.il/ClicksStatistics/entitlementsServicesV2.jsp?et=ngt&lp=/direct/hls/live/2033791/k12dvr/index.m3u8?b-in-range=800-2700&rv=AKAMAI'
    )
    const ticket = data.tickets[0].ticket
    channelsTabs.value[1].link = `https://mako-streaming.akamaized.net/direct/hls/live/2033791/k12dvr/index.m3u8?${ticket}`
  } catch (error) {
    console.error('Error fetching ticket:', error)
  }
}

onMounted(async () => {
  updateTicket()
  updateTicketId.value = setInterval(updateTicket, 5 * 60 * 1000)
})

onBeforeUnmount(() => {
  clearInterval(updateTicketId.value)
})
</script>

<template>
  <v-tabs v-model="currentChannelTab" align-tabs="center" center-active grow dir="rtl">
    <v-tab v-for="channelTab in channelsTabs" :key="channelTab.title" :value="channelTab">
      {{ channelTab.title }}
    </v-tab>
  </v-tabs>
</template>

<style scoped>
.v-tab {
  text-transform: none !important;
}
</style>
