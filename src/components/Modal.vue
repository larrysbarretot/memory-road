<template>
  <div class="text-center">
    <v-dialog
      v-model="modal.dialog"
      max-width="650"
    >
      <v-card>
        <h2 class="pa-2 text-center" :class="{'display-2': !isMobil, 'display-1': isMobil}" v-if="modal.turn">TURNO DE:</h2>
        <h2 class="display-2 pa-2 text-center" v-if="modal.isWinner">GANADOR</h2>
        <h2 class="pa-2 text-center" :class="{'display-3': !isMobil, 'display-1': isMobil}">{{ modal.title }}</h2>
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
    },
    isMobil() {
      return this.$vuetify.breakpoint.xs;
    }
  }
}
</script>

<style>
  
</style>