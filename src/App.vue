<script setup lang="ts">
import { ref } from 'vue'
import VideoPlayer from './components/VideoPlayer.vue'

type Tab = {
  id: number
  title: string
  link: string
}

const tabs: Tab[] = [
  {
    id: 1,
    title: 'ערוץ 11',
    link: 'https://kan11w.media.kan.org.il/hls/live/2105694/2105694/source1_4k/chunklist.m3u8'
  },
  {
    id: 2,
    title: 'ערוץ 12',
    link: 'https://mako-streaming.akamaized.net/direct/hls/live/2033791/k12dvr/index.m3u8?hdnea=st%3D1703756539%7Eexp%3D1703757439%7Eacl%3D%2F*%7Ehmac%3D59fa4d2f5b9f7be40ee812f42360d7ca036bc190f75f8f7ad5fb441a0a4101be'
  },
  {
    id: 3,
    title: 'ערוץ 13',
    link: 'https://reshet.g-mana.live/media/87f59c77-03f6-4bad-a648-897e095e7360/profile/2/profileManifest.m3u8'
  },
  {
    id: 4,
    title: 'ערוץ 14',
    link: 'https://now14.g-mana.live/media/91517161-44ab-4e46-af70-e9fe26117d2e/mainManifest.m3u8'
  }
]

const currentTab = ref<Tab>(tabs[3])
</script>

<template>
  <v-app>
    <v-app-bar color="blue" app>
      <v-tabs v-model="currentTab" grow dir="rtl">
        <v-tab v-for="tab in tabs" :key="tab.id" :value="tab">
          {{ tab.title }}
        </v-tab>
      </v-tabs>
    </v-app-bar>
    <v-main>
      <v-container>
        <VideoPlayer :video-link="currentTab.link" />
      </v-container>
    </v-main>
  </v-app>
</template>

<style scoped></style>
