<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
        />

        <v-toolbar-title>LAB</v-toolbar-title>
      </div>
      <v-spacer></v-spacer>
      <v-btn
        class="mx-2"
        fab
        dark
        small
        color="teal"
        v-if="play"
        @click="restart">
        <v-icon>mdi-restart</v-icon>
      </v-btn>
      <v-spacer></v-spacer>

      <v-switch
        v-model="showConfig"
        hide-details
        inset
        :disabled="play"
      >
        <template v-slot:label>
          <v-icon>mdi-settings-outline</v-icon>
        </template>
      </v-switch>
    </v-app-bar>

    <v-content>
      <Configuration
        v-if="showConfig"
        :config="config"
        :players="players" />
      <Home
        v-if="!showConfig && !play"
        @playGame="started"/>
      <Board
        v-if="play"
        :squares="squares"
        :players="players"
        :showPlayers="config.showPlayers"
        @stepPressed="stepped" />
      <Modal :modal="modal" />
    </v-content>
  </v-app>
</template>

<script>
import Configuration from './components/Configuration';
import Home from './components/Home';
import Board from './components/Board';
import Modal from './components/Modal';

export default {
  name: 'App',

  components: {
    Home,
    Configuration,
    Board,
    Modal
  },

  data: () => ({
    config: {
      board: {
        min: 1,
        max: 20,
        rows: 6
      },
      showPlayers: false
    },
    showConfig: false,
    players: {
      1: {
        name: 'Elí',
        color: '#82F70C',
        position: {
          x: -1,
          y: -1
        },
        steps: 0
      },
      2: {
        name: 'Larry',
        color: '#F7B50C',
        position: {
          x: -1,
          y: -1
        },
        steps: 0
      }
    },
    play: false,
    squares: [],
    turn: true,
    modal: {
      dialog: false,
      title: '',
      image: ''
    },
    correctImages: [
      '3gIIVTrVoacyBdLtfh',
      '8m7QYk0LVfQ1n4cXbc',
      'TEUxtCqq5aa2s',
      'AgrfqPt5AyiTm'
    ],
    incorrectImages: [
      '3s298sv3aevOC4fktQ',
      'Xb7VXx9yEzvtlFEM71',
      'S9Egk23jFzLLtfur56',
      'BcPfTUCqYZSL5NZC0p',
      'bnN5dDIiuuJ4A'
    ]
  }),
  methods: {
    started(e) {
      this.play = e;
      this.buildRoad();
      this.backgroundSound();
    },
    buildRoad() {
      let choice = Math.floor(Math.random() * 3);
      for (let row = this.config.board.rows - 1; row >= 0; row--) {
        for (let column = 0; column < 3; column++) {
          let square = {};
          square.x = row;
          square.y = column;
          square.isSafe = false;
          square.color = "#B9F6CA";
          if (choice === column) square.isSafe = true;
          this.squares.push(square);
        }
        choice = this.choiceSafeSquare(choice, 3);
      }
    },
    choiceSafeSquare(previus, columns) {
      let min = previus - 1;
      let max = previus + 1;
      if (min < 0) min = 0;
      if (max > columns - 1) max = columns - 1;
      return Math.floor(Math.random() * (max + 1 - min) + min);
    },
    stepped(square) {
      const player = this.turn ? this.players[1]: this.players[2];
      const currentColor = square.color;
      if (player.position.x === square.x - 1) {
        /* eslint-disable no-console */
        console.log(`Avance permitido`);
        /* eslint-enable no-console */
        if (square.isSafe) {
          square.color = 'blue darken-1';
          this.correctSound();
          this.forward(player, square);
          if (this.playerWin(player)) {
            this.setupModal(`'${player.name}' es el GANADOR`, this.randomImage(this.correctImages));
          } else {
            this.setupModal('¡¡CORRECTO!!', this.randomImage(this.correctImages));
            this.blinkModal(this.modal);
          }
        } else {
          square.color = 'red darken-1';
          this.resetPosition(player);
          this.incorrectSound();
          this.setupModal('¡¡INCORRECTO!!', this.randomImage(this.incorrectImages));
          this.blinkModal(this.modal);
          this.turn = !this.turn;
          this.setupModal(`TURNO de ${this.turn ? this.players[1].name: this.players[2].name}`, '')
        }
        this.blinkColor(square, currentColor);
      } else {
        /* eslint-disable no-console */
        square.color = 'red darken-1';
        this.incorrectSound();
        this.setupModal('NO ESTÁ PERMITIDO!!', this.randomImage(this.incorrectImages));
        this.blinkModal(this.modal);
        this.blinkColor(square, currentColor);
        console.log(`Avance no permitido`);
        /* eslint-enable no-console */
      }
      /* eslint-disable no-console */
      console.log(`App: (${square.x}, ${square.y})`);
      /* eslint-enable no-console */
    },
    blinkColor(square, color) {
      setTimeout(() => {
          square.color = color;
        }, 1500);
    },
    forward(player, square) {
      player.position.x = square.x;
      player.position.y = square.y;
      player.steps++;
    },
    correctSound() {
      const correct = new Audio(require('@/assets/sounds/correct.mp3'));
      correct.play();
    },
    incorrectSound() {
      const error = new Audio(require('@/assets/sounds/error.mp3'));
      error.play();
    },
    backgroundSound() {
      const background = new Audio(require('@/assets/sounds/background.mp3'));
      background.play();
    },
    randomImage(array) {
      return array[Math.floor(Math.random() * array.length)];
    },
    blinkModal(modal) {
      setTimeout(() => {
        modal.dialog = false;
      }, 2500);
    },
    setupModal(title, image) {
      this.modal.dialog = true;
      this.modal.title = title;
      this.modal.image = image;
    },
    resetPosition(player) {
      player.position.x = -1;
      player.position.y = -1;
      player.steps = 0;
    },
    playerWin(player) {
      return (player.position.x === this.config.board.rows - 1) && (player.steps === this.config.board.rows);
    },
    restart() {
      this.squares = [];
      this.resetPosition(this.players[1]);
      this.resetPosition(this.players[2]);
      this.started(true);
    }
  }
};
</script>
