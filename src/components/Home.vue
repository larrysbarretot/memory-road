<template>
  <v-container mt-12>
    <v-layout
      text-center
      wrap
    >
      <v-flex xs12>
        <v-img
          :src="require('@/assets/dory.png')"
          class="my-5 bounce-out-top"
          contain
          height="200"
        ></v-img>
      </v-flex>

      <v-flex mb-4>
        <h2 class="display-2 font-weight-bold mb-3">
          MEMORY ROAD
        </h2>
        <div class="my-2">
          <v-btn rounded x-large color="info" dark @click="playGame">
            JUGAR
            <v-icon right dark>mdi-play</v-icon>
          </v-btn>
        </div>
        <p class="subheading font-weight-regular">
          Buscando y recordando el camino de la victoria.
          <br>By
          <a href="https://ibbnaranjal.com" target="_blank">ibbnaranajal.com</a>
        </p>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
export default {
  name: 'HelloWorld',

  data: () => ({
    
  }),
  methods: {
    playGame() {
      this.$emit('playGame', true);
    }
  }
};
</script>

<style>
.bounce-out-top {
  animation: bounce-out-top 1.5s infinite both;
}

@-webkit-keyframes bounce-out-top {
  0% {
    -webkit-transform: translateY(0);
            transform: translateY(0);
    -webkit-animation-timing-function: ease-out;
            animation-timing-function: ease-out;
  }
  5% {
    -webkit-transform: translateY(-30px);
            transform: translateY(-30px);
    -webkit-animation-timing-function: ease-in;
            animation-timing-function: ease-in;
  }
  15% {
    -webkit-transform: translateY(0);
            transform: translateY(0);
    -webkit-animation-timing-function: ease-out;
            animation-timing-function: ease-out;
  }
  25% {
    -webkit-transform: translateY(-38px);
            transform: translateY(-38px);
    -webkit-animation-timing-function: ease-in;
            animation-timing-function: ease-in;
  }
  38% {
    -webkit-transform: translateY(0);
            transform: translateY(0);
    -webkit-animation-timing-function: ease-out;
            animation-timing-function: ease-out;
  }
  52% {
    -webkit-transform: translateY(-75px);
            transform: translateY(-75px);
    -webkit-animation-timing-function: ease-in;
            animation-timing-function: ease-in;
  }
  70% {
    -webkit-transform: translateY(0);
            transform: translateY(0);
    -webkit-animation-timing-function: ease-out;
            animation-timing-function: ease-out;
  }
  85% {
    opacity: 1;
  }
  100% {
    -webkit-transform: translateY(-800px);
            transform: translateY(-800px);
    opacity: 0;
  }
}
@keyframes bounce-out-top {
  0% {
    -webkit-transform: translateY(0);
            transform: translateY(0);
    -webkit-animation-timing-function: ease-out;
            animation-timing-function: ease-out;
  }
  5% {
    -webkit-transform: translateY(-30px);
            transform: translateY(-30px);
    -webkit-animation-timing-function: ease-in;
            animation-timing-function: ease-in;
  }
  15% {
    -webkit-transform: translateY(0);
            transform: translateY(0);
    -webkit-animation-timing-function: ease-out;
            animation-timing-function: ease-out;
  }
  25% {
    -webkit-transform: translateY(-38px);
            transform: translateY(-38px);
    -webkit-animation-timing-function: ease-in;
            animation-timing-function: ease-in;
  }
  38% {
    -webkit-transform: translateY(0);
            transform: translateY(0);
    -webkit-animation-timing-function: ease-out;
            animation-timing-function: ease-out;
  }
  52% {
    -webkit-transform: translateY(-75px);
            transform: translateY(-75px);
    -webkit-animation-timing-function: ease-in;
            animation-timing-function: ease-in;
  }
  70% {
    -webkit-transform: translateY(0);
            transform: translateY(0);
    -webkit-animation-timing-function: ease-out;
            animation-timing-function: ease-out;
  }
  85% {
    opacity: 1;
  }
  100% {
    -webkit-transform: translateY(-800px);
            transform: translateY(-800px);
    opacity: 0;
  }
}

</style>
