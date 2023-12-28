<script setup lang="ts">
import {reactive,ref} from "vue"
const tabs = reactive([
        { title: "Tab 1" },
        { title: "Tab 2" },
        { title: "Tab 3" },
      ])
      const tab = ref(null)
      console.log(tab.value);
      
</script>

<template>
  <v-app>
    <v-app-bar app>
      <v-tabs v-model="tab" bg-color="primary">
        <v-tab v-for="(tab, index) in tabs" :key="index">
          {{ tab.title }}
        </v-tab>
      </v-tabs>
    </v-app-bar>
  </v-app>
</template>

<style scoped>

</style>
