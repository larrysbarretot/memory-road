<template>
  <v-container>
    <v-row>
      <v-col>
        <p class="display-4 font-italic text-center flip-2-hor-top-1 meta">M</p>
      </v-col>
      <v-col>
        <p class="display-4 font-italic text-center flip-2-hor-top-1 meta">E</p>
      </v-col>
      <v-col>
        <p class="display-4 font-italic text-center flip-2-hor-top-1 meta">T</p>
      </v-col>
      <v-col>
        <p class="display-4 font-italic text-center flip-2-hor-top-1 meta">A</p>
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
      this.$emit('stepPressed', square);
    },
    isHere(square, number) {
      return (this.players[number].position.x === square.x) && (this.players[number].position.y === square.y);
    }
  }
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css?family=Indie+Flower&display=swap');

.meta {
  color: #E72027;
}
p.display-4.meta {
  font-family: 'Indie Flower', cursive !important;
}
.flip-2-hor-top-1 {
  animation: flip-2-hor-top-1 0.6s cubic-bezier(0.455, 0.030, 0.515, 0.955) 0.4s infinite both;
}

@-webkit-keyframes flip-2-hor-top-1 {
  0% {
    -webkit-transform: translateY(0) rotateX(0);
            transform: translateY(0) rotateX(0);
    -webkit-transform-origin: 50% 0%;
            transform-origin: 50% 0%;
  }
  100% {
    -webkit-transform: translateY(-100%) rotateX(-180deg);
            transform: translateY(-100%) rotateX(-180deg);
    -webkit-transform-origin: 50% 100%;
            transform-origin: 50% 100%;
  }
}
@keyframes flip-2-hor-top-1 {
  0% {
    -webkit-transform: translateY(0) rotateX(0);
            transform: translateY(0) rotateX(0);
    -webkit-transform-origin: 50% 0%;
            transform-origin: 50% 0%;
  }
  100% {
    -webkit-transform: translateY(-100%) rotateX(-180deg);
            transform: translateY(-100%) rotateX(-180deg);
    -webkit-transform-origin: 50% 100%;
            transform-origin: 50% 100%;
  }
}

</style>