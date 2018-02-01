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

const MP_URL = MP_BASE_URL + '/search.json?expanded=true&q=tags%3Averanstaltung%20status%3Aopen%20order%3Alatest'


export default {
  name: 'MPEvents',
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
      return 'Veranstaltungen'
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
