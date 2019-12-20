<template>
  <div class="text-center">
    <v-dialog
      v-model="modal.dialog"
      width="500"
    >
      <v-card>
        <!-- <v-card-title
          class="headline grey lighten-2"
          primary-title
        > -->
        <h2 class="display-3 pa-2 text-center">{{ modal.title }}</h2>
          
        <!-- </v-card-title> -->

        <v-card-text>
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

</style>