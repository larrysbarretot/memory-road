<template>
  <v-container>
    <v-row>
      <v-col>
        test
      </v-col>
    </v-row>
    <v-row no-gutters>
      <Square
        v-for="(square, index) in squares"
        :key="index"
        :square="square"
        @selected="stepPressed">
        <template v-if="showPlayers">
        <Player v-if="isHere(square, 1)" :player="players[1]"/>
        <Player v-if="isHere(square, 2)" :player="players[2]"/>
        </template>
      </Square>
    </v-row>
  </v-container>
</template>

<script>
import Square from './Square';
import Player from './Player';

export default {
  props: ['squares', 'players', 'showPlayers'],
  components: {
    Square,
    Player
  },
  methods: {
    stepPressed(square) {
      /* eslint-disable no-console */
      //console.log(`Board: (${square.x}, ${square.y})`);
      /* eslint-enable no-console */
      this.$emit('stepPressed', square);
    },
    isHere(square, number) {
      return (this.players[number].position.x === square.x) && (this.players[number].position.y === square.y);
    }
  }
}
</script>

<style>

</style>