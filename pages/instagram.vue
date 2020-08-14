<template>
  <v-layout>
    <v-flex class="text-center">
      <v-icon size="100pt">
        mdi-instagram
      </v-icon>
    </v-flex>
    <v-flex>
      <v-card>
        <v-card-title class="headline">
          Let's get some data
        </v-card-title>
        <v-card-actions>
          <v-btn
            title="Clear"
            @click="clear"
          >
            X
          </v-btn>
          <v-btn
            title="Go get the data"
            color="primary"
            @click="getInstagramFakeData"
          >
            Fake
          </v-btn>
        </v-card-actions>
        <v-card-text>
          <json-view :data="result" />
        </v-card-text>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
import { JSONView } from 'vue-json-component'
import instagram from '../assets/instagram'

export default {

  components: { 'json-view': JSONView },

  data () {
    return {
      result: {},
    }
  },

  methods: {
    clear () {
      this.result = {}
    },
    getInstagramFakeData () {
      this.result = instagram
    },
  },
}
</script>
