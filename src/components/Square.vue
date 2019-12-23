<template>
  <v-col cols="4">
    <v-card
      class="pa-3 ma-1 square"
      height=120
      :color="square.color"
      @click="selected"
    >
      <!-- {{ square.x }} {{ square.y }} -->
      <slot></slot>
    </v-card>
  </v-col>
</template>

<script>
export default {
  props: ['square'],
  methods: {
    selected() {
      /* eslint-disable no-console */
      //console.log(`Seleccionado: (${this.square.x}, ${this.square.y})`);
      /* eslint-enable no-console */
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