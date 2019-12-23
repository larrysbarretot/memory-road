<template>
  <v-col cols="4">
    <v-card
      class="pa-3 ma-1 square"
      height=120
      :color="square.color"
      @click="selected"
    >
      <slot></slot>
    </v-card>
  </v-col>
</template>

<script>
export default {
  props: ['square'],
  methods: {
    selected() {
      this.$emit('selected', this.square);
    }
  }
}
</script>

<style scoped>
  .square {
    display: flex;
    justify-content: center;
    align-items: center;
  }
</style>