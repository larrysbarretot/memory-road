<template>
  <v-container>
    <v-layout
      text-center
      wrap
    >
      <v-flex xs12>
        <v-img
          :src="require('../assets/dory.png')"
          class="my-3"
          contain
          height="200"
        ></v-img>
      </v-flex>

      <v-flex mb-4>
        <h1 class="display-2 font-weight-bold mb-3">
          MEMORY ROAD
        </h1>
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
      /* eslint-disable no-console */
      this.$emit('playGame', true);
      console.log('play');
      /* eslint-enable no-console */
    }
  }
};
</script>
