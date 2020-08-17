<template>
  <v-layout>
    <v-card class="mx-auto">
      <v-app-bar>
        <v-toolbar-title>
          <v-icon size="xx-large">
            mdi-instagram
          </v-icon>
        </v-toolbar-title>
        <v-spacer />
        <v-btn
          title="Clear"
          @click="clear"
        >
          X
        </v-btn>
        <v-btn
          title="Go get the fake data"
          color="primary"
          @click="getInstagramFakeData"
        >
          Fake
        </v-btn>
        <v-btn
          title="Go get the real data"
          color="success"
          @click="getInstagramAPIData"
        >
          Fetch
        </v-btn>
      </v-app-bar>
      <v-container>
        <v-row dense>
          <v-col cols="12">
            <json-view :data="result" />
          </v-col>
        </v-row>
      </v-container>
    </v-card>
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
    async getInstagramAPIData () {
      const query = {
        short_code: 'BzinsTanXXv', // use any shortcode you want
      }

      const url = new URL('https://instagram28.p.rapidapi.com/media_likers')
      url.search = new URLSearchParams(query).toString()

      const headers = {
        'x-rapidapi-host': 'instagram28.p.rapidapi.com',
        'x-rapidapi-key': '[PUT YOUR API KEY HERE]',
      }

      const response = await fetch(url.toString(), { headers })
      this.result = await response.json()
    },
  },
}
</script>
