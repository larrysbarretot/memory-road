<template>
  <v-avatar class="player" :style="{backgroundColor: player.color}" size="80">
    <span class="white--text headline">{{ player.name }}</span>
  </v-avatar>
</template>

<script>
export default {
  props: ['player']
}
</script>

<style scoped>
  .player {
    border-radius: 100% !important;
  }
</style>