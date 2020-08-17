<template>
  <v-layout>
    <v-card :loading="loading" class="mx-auto">
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
          <v-col
            v-for="(edge, i) in edges"
            :key="i"
            cols="12"
          >
            <v-card>
              <div class="d-flex flex-no-wrap justify-space-between">
                <div>
                  <v-card-title
                    class="headline"
                    v-text="edge.node.full_name"
                  />
                  <v-card-subtitle v-text="edge.node.username" />
                  <v-chip filter :disabled="!edge.node.is_private" :input-value="edge.node.is_private"> Private </v-chip>
                  <v-chip filter :disabled="!edge.node.is_verified" :input-value="edge.node.is_verified"> Verified </v-chip>
                  <v-chip filter :disabled="!edge.node.followed_by_viewer" :input-value="edge.node.followed_by_viewer"> Followed </v-chip>
                  <v-chip filter :disabled="!edge.node.requested_by_viewer" :input-value="edge.node.requested_by_viewer"> Requested </v-chip>
                </div>
                <v-avatar class="ma-3" size="125">
                  <v-img :src="edge.node.profile_pic_url" />
                </v-avatar>
              </div>
            </v-card>
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
      loading: false,
    }
  },

  computed: {
    edges () {
      console.log(this.result)
      if (
        this.result &&
        this.result.data &&
        this.result.data.shortcode_media &&
        this.result.data.shortcode_media.edge_liked_by
      ) {
        return this.result.data.shortcode_media.edge_liked_by.edges
      } else {
        return []
      }
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
      this.loading = true

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
      this.loading = false
    },
  },
}
</script>
