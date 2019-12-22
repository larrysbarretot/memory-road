<template>
  <div class="text-center">
    <v-dialog
      v-model="modal.dialog"
      max-width="650"
      :content-class="modal.turn ? 'turn': undefined"
    >
      <v-card>
        <h2 class="display-2 pa-2 text-center" v-if="modal.turn">TURNO DE:</h2>
        <h2 class="display-2 pa-2 text-center" v-if="modal.isWinner">GANADOR</h2>
        <h2 class="display-3 pa-2 text-center">{{ modal.title }}</h2>
        <v-card-text v-if="!modal.turn" class="pa-0">
          <div v-if="modal.image" style="width:100%;height:0;padding-bottom:100%;position:relative;">
            <iframe :src="url" width="100%" height="100%" style="position:absolute" frameBorder="0" class="giphy-embed" allowFullScreen></iframe>
          </div>
        </v-card-text>
    
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  props: ['modal'],
  computed: {
    url() {
      return `https://giphy.com/embed/${this.modal.image}`;
    }
  }
}
</script>

<style>
  .turn {
    position: absolute;
    bottom: 50px;
    margin: 0 auto !important;
  }
</style>