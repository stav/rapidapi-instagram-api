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
