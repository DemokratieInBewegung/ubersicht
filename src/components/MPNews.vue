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

const MP_URL = MP_BASE_URL + '/search.json?expanded=true&q=category:13 after:{} order:latest_topic'


export default {
  name: 'MPNews',
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
      if (this.unseen.length) {
        return 'Marktplatz News <b-badge>' + this.unseen.legth + '</b-bade>'
      }
      return 'Marktplatz News'
    }
  },
  methods: {
    refreshItems () {
      fetch(MP_URL, {
        // method: "GET",
        // headers: new Headers({
        //   'Accept': 'application/json',
        //   'X-Requested-With': 'XMLHttpRequest'
        // })
      }
      ).then(x => x.json()
      ).then(x => x.topics
      ).then(data => { this.items = data; console.log(data) }
      )
    }
  }
}
</script>
