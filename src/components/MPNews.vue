<template>
  <b-card header-tag="header">
    <card-header slot="header" title="Marktplatz News" :badge="unseen.length" />
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
import CardHeader from './CardHeader'

const MP_URL = MP_BASE_URL + '/c/informationen-und-aktuelles.json'

export default {
  name: 'MPNews',
  components: {
    MPTopicLink,
    CardHeader
  },
  mounted () {
    this.refreshItems()
  },
  data () {
    return {items: [], users: []}
  },
  computed: {
    selected () {
      return this.items.slice(0, 10)
    },
    unseen () {
      return this.selected.filter(x => x.unseen)
    }
  },
  methods: {
    refreshItems () {
      fetch(MP_URL, {
        credentials: 'include'
      }
      ).then(x => x.json()
      ).then(x => {
        this.items = x.topic_list.topics
        this.users = x.users
      })
    }
  }
}
</script>
