<template>
  <b-card :header="header">
    <ul class="list-unstyled">
      <li v-for="topic in selected" :key="topic.id">
        <MPTopicLink v-bind:topic="topic" />
      </li>
    </ul>
  </b-card>
</template>

<script>

import { MP_BASE_URL } from '../consts.js'
import MPTopicLink from './MPTopicLink'

const MP_URL = MP_BASE_URL + '/search.json?expanded=true&q=in:tracking in:unseen in:watching order:latest'

export default {
  name: 'MPMine',
  components: {
    MPTopicLink
  },
  mounted () {
    this.refreshItems()
  },
  data () {
    return {items: []}
  },
  computed: {
    selected () {
      return this.items.slice(0, 10)
    },
    unseen () {
      return this.selected.filter(x => x.unseen)
    },
    header () {
      return 'Mein Marktplatz Themen'
    }
  },
  methods: {
    refreshItems () {
      fetch(MP_URL, {
        credentials: 'include'
      }
      ).then(x => x.json()
      ).then(x => x.topics
      ).then(data => { this.items = data }
      )
    }
  }
}
</script>
