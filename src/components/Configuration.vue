<template>
  <v-container>
    <v-layout
      text-center
      wrap
    >
      <v-flex mb-4>
        <h2 class="display-1 font-weight-bold mb-3">
          Configuración del Juego
        </h2>
        <v-form
          ref="form"
        >
          <v-subheader class="pl-0">Filas</v-subheader>
          <v-slider
            v-model="config.board.rows"
            class="align-center"
            :max="config.board.max"
            :min="config.board.min"
            hide-details
            thumb-label="always"
          >
          </v-slider>

          <v-subheader class="pl-0">Tiempo que se muestra la imagen. (1 segundo = 1000)</v-subheader>
          <v-slider
            v-model="modal.time"
            class="align-center"
            max="5000"
            min="1000"
            hide-details
            thumb-label="always"
          >
          </v-slider>

          <v-switch
            v-model="config.showPlayers"
            :label="`Mostrar Jugadores`"
          ></v-switch>

          <v-layout>
            <v-flex md6 class="mx-auto">
              <v-card
                class="mx-auto my-2 pa-2"
                
                outlined
              >
                <v-text-field
                  v-model="players[1].name"
                  label="Nombre del Jugador 1"
                  outlined
                ></v-text-field>

                <v-color-picker
                  v-model="players[1].color"
                  :hide-canvas="true"
                  :hide-inputs="false"
                  :hide-mode-switch="true"
                  :mode.sync="mode"
                  :show-swatches="false"
                  class="mx-auto"
                ></v-color-picker>
              </v-card>
            </v-flex>
          </v-layout>
          <v-layout>
            <v-flex md6 class="mx-auto">
              <v-card
                class="mx-auto my-2 pa-2"
                
                outlined
              >
                <v-text-field
                  v-model="players[2].name"
                  label="Nombre del Jugador 2"
                  outlined
                ></v-text-field>

                <v-color-picker
                  v-model="players[2].color"
                  :hide-canvas="true"
                  :hide-inputs="false"
                  :hide-mode-switch="true"
                  :mode.sync="mode"
                  :show-swatches="false"
                  class="mx-auto"
                ></v-color-picker>
              </v-card>
            </v-flex>
          </v-layout>
        </v-form>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
export default {
  props: ['config', 'players', 'modal'],
  data() {
    return {
      mode: 'hexa'
    }
  }
}
</script>