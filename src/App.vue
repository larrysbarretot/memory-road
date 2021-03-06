<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <v-btn
          tile
          large
          icon
          href='./'>
          <v-img
            alt="Vuetify Logo"
            class="shrink mr-2"
            contain
            src="@/assets/lab-logo.png"
            transition="scale-transition"
            width="65"
          />
        </v-btn>
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
        :players="players"
        :modal="modalMeme" />
      <Home
        v-if="!showConfig && !play"
        @playGame="started"/>
      <Board
        v-if="play"
        :squares="squares"
        :players="players"
        :showPlayers="config.showPlayers"
        @stepPressed="stepped" />
      <Modal :modal="modalMeme" />
      <Modal :modal="modalTurn" />
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
        color: '#0CB2F7',
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
    modalMeme: {
      dialog: false,
      title: '',
      image: '',
      turn: false,
      isWinner: false,
      time: 4000
    },
    modalTurn: {
      dialog: false,
      title: '',
      image: '',
      turn: false,
      isWinner: false,
      time: 1700
    },
    correctImages: [
      '3gIIVTrVoacyBdLtfh',
      '8m7QYk0LVfQ1n4cXbc',
      'TEUxtCqq5aa2s',
      'AgrfqPt5AyiTm',
      'XbxZ41fWLeRECPsGIJ',
      'fLpP1CZyzFckyy5bML',
      '8Fxlp25ov2RCGwye9I',
      'QJsP0cTAJhg7O47ub2',
      '2aPePPqpAf5jwjtt2p',
      'TgFibAJdezKXUYsddv',
      'Nmiaz28CS7T3jhIe9K',
      'MfKSNCNqudSE0',
      'X8czYMb8gPPPu3USXv',
      '4Nrxg6Cp6u9MMkxKgu',
      'S2w7v6v1Qmy2nusJO5',
      '1Bg837HL6HyjiJWp3R',
      'bznNJlqAi4pBC',
      'iAm3b28WTqaHe',
      'dXhxtHSlo95MlfMR9j',
      'Z5qJVWJSXdGzm',
      'zpZMHnDdTS4PS',
      '10xZU9b7JBx14s',
      'ZU9QbQtuI4Xcc',
      '11ISwbgCxEzMyY',
      'diUKszNTUghVe',
      'RIhNQOjGa39Ze',
      '1gqDQUaLe3mCc',
      'VHwgHhJLuWt0gjjUzf',
      'LML5ldpTKLPelFtBfY',
      'Loyn98mvzdWfe',
      'w4hyiTjWa9iXm',
      'uTY9YvXryUtpaNaDiG',
      'l44Q6Etd5kdSGttXa'
    ],
    incorrectImages: [
      '3s298sv3aevOC4fktQ',
      'Xb7VXx9yEzvtlFEM71',
      'S9Egk23jFzLLtfur56',
      'BcPfTUCqYZSL5NZC0p',
      'bnN5dDIiuuJ4A',
      'gvOqQdFq9unPG',
      'ZrkWh5DCKQPTy',
      'l2JhtKtDWYNKdRpoA',
      'loKoRwzBQi1K8',
      'fmJwUINtn2Odi',
      'sT1K7rkPJOwh2',
      'cvVIHUOKtYeNW',
      '14nvd5FCcDpFlu',
      'sUzZwE9AgI8iA',
      '6xgslyYQCyLa8',
      'pScTZ7mTJ1Udi',
      '5N6FWdDKKyqOI',
      'l0HlCqV35hdEg2GUo',
      '7QxZfWnBLdGI8',
      '7WlEkydlQEi0o',
      'jquDWJfPUMCiI',
      'zXeX29w6jxjAk'
    ],
    audioOn: true,
    audio: null,
  }),
  methods: {
    started(e) {
      this.play = e;
      this.buildRoad();
      this.backgroundSound();
      this.setupModalTurn(`${this.turn ? this.players[1].name: this.players[2].name}`, '', true, false);
      this.blinkModal(this.modalTurn, this.modalTurn.time);
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
        if (square.isSafe) {
          square.color = 'blue darken-1';
          this.correctSound();
          this.forward(player, square);
          if (this.playerWin(player)) {
            this.setupModalMeme(`'${player.name}'`, this.randomImage(this.correctImages), false, true);
            if (this.audioOn && this.audio) {
              this.audio.pause();
              this.winnerSound();
            }
          } else {
            this.setupModalMeme('¡¡CORRECTO!!', this.randomImage(this.correctImages), false, false);
            this.blinkModal(this.modalMeme, this.modalMeme.time);
          }
        } else {
          square.color = 'red darken-1';
          this.resetPosition(player);
          this.incorrectSound();
          this.setupModalMeme('¡¡INCORRECTO!!', this.randomImage(this.incorrectImages), false, false);
          this.blinkModal(this.modalMeme, this.modalMeme.time);
          this.turn = !this.turn;
          let vm = this;
          setTimeout(() => {
            vm.setupModalTurn(`${vm.turn ? vm.players[1].name: vm.players[2].name}`, '', true, false);
            vm.blinkModal(vm.modalTurn, vm.modalTurn.time);
          }, this.modalMeme.time);
        }
        this.blinkColor(square, currentColor);
      } else {
        square.color = 'red darken-1';
        this.incorrectSound();
        this.setupModalMeme('NO ESTÁ PERMITIDO!!', this.randomImage(this.incorrectImages), false, false);
        this.blinkModal(this.modalMeme, this.modalMeme.time);
        this.blinkColor(square, currentColor);
      }
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
      if (this.audio) {
        this.audio.pause();
      }
      const numberError = Math.floor(Math.random() * 12 + 1);
      const error = new Audio(require(`@/assets/sounds/error${numberError}.mp3`));
      error.play();
      let vm = this;
      setTimeout(() => {
        vm.audio.play();
        error.pause();
      }, this.modalMeme.time);
    },
    winnerSound() {
      const numberWinner = Math.floor(Math.random() * 3 + 1);
      let winner = new Audio(require(`@/assets/sounds/win${numberWinner}.mp3`));
      let youwin = new Audio(require('@/assets/sounds/youwin.mp3'));
      youwin.play();
      winner.play();
    },
    backgroundSound() {
      this.audio = new Audio(require('@/assets/sounds/background.mp3'));
      this.audio.loop = true;
      this.audio.play();
    },
    randomImage(array) {
      return array[Math.floor(Math.random() * array.length)];
    },
    blinkModal(modal, time) {
      setTimeout(() => {
        modal.dialog = false;
      }, time);
    },
    setupModalMeme(title, image, turn, isWinner) {
      this.modalMeme.dialog = true;
      this.modalMeme.title = title;
      this.modalMeme.image = image;
      this.modalMeme.turn = turn;
      this.modalMeme.isWinner = isWinner;
    },
    setupModalTurn(title, image, turn, isWinner) {
      this.modalTurn.dialog = true;
      this.modalTurn.title = title;
      this.modalTurn.image = image;
      this.modalTurn.turn = turn;
      this.modalTurn.isWinner = isWinner;
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
      this.audio.pause();
      this.started(true);
    }
  }
};
</script>
