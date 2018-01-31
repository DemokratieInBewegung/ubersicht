<template>
  <b-card header-tag="header" footer-tag="footer">
    <card-header slot="header" :title="headerTitle" :link="catLink" :badge="unseen.length" />
    <ul class="list-unstyled">
      <li v-for="topic in selected" :key="topic.id">
        <MPTopicLink v-bind:topic="topic" />
      </li>
    </ul>
    <card-footer v-if="cat.subcats" slot="footer">
      <MPCatLink v-for="c in cat.subcats" :cat="c" :key="c.id" />
    </card-footer>
  </b-card>
</template>

<script>

import { MP_BASE_URL } from '../consts.js'
import MPTopicLink from './MPTopicLink'
import MPCatLink from './MPCatLink'
import CardHeader from './CardHeader'

export default {
  name: 'MPCategory',
  props: ['cat', 'title', 'allCats'],
  components: {
    MPTopicLink,
    CardHeader,
    MPCatLink
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
    catLink () {
      if (this.cat.parent_category_id) {
        return `${MP_BASE_URL}/c/${this.allCats[this.cat.parent_category_id].slug}/${this.cat.slug}`
      }
      return `${MP_BASE_URL}/c/${this.cat.slug}`
    }
  },
  methods: {
    refreshItems () {
      fetch(this.catLink + '.json', {
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
