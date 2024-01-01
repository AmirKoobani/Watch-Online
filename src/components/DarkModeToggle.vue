<script setup lang="ts">
import { useTheme } from 'vuetify'

const theme = useTheme()

const toggleTheme = () => {
  theme.global.name.value = theme.global.current.value.dark ? 'light' : 'dark'
}
</script>

<template>
  <v-btn icon size="large" :elevation="0" @click="toggleTheme">
    <span v-if="theme.global.current.value.dark" class="icon">🌞</span>
    <span v-else class="icon">🌛</span>
  </v-btn>
</template>

<style scoped>
.icon {
  font-size: 2em;
}
</style>
