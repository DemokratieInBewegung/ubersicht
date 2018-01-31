<template>
  <b-card>
    <card-header slot="header" title="PN Eingang" :link="link" :badge="unseen.length" />
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

export default {
  name: 'MPMessages',
  props: ['user'],
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
    headerTitle () {
      return this.title || this.cat.name
    },
    unseen () {
      return this.selected.filter(x => x.unseen)
    },
    link () {
      return `${MP_BASE_URL}/u/${this.user.username}/messages`
    }
  },
  methods: {
    refreshItems () {
      fetch(`${MP_BASE_URL}/topics/private-messages/${this.user.username}.json`, {
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
