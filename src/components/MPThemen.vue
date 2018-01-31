<template>
  <b-card header-tag="header" footer-tag="footer">
    <card-header slot="header" :title="headerTitle" :link="catLink" :badge="unseen.length" />
    <ul class="list-unstyled list-inline" v-if="cat.subcats">
      <li class="list-inline-item" v-for="c in kreise"  :key="c.id">
        <MPCatLink badge="1" :cat="c" />
      </li>
    </ul>
    <span class="text-uppercase">Werden diskutiert</span>
    <ul class="list-unstyled">
      <li v-for="topic in selected" :key="topic.id">
        <MPTopicLink v-bind:topic="topic" />
      </li>
    </ul>
  </b-card>
</template>

<script>

import { MP_BASE_URL, MP_THEMES_CAT_ID, MP_THEMES_NEW_CAT_ID } from '../consts.js'
import MPTopicLink from './MPTopicLink'
import MPCatLink from './MPCatLink'
import CardHeader from './CardHeader'

export default {
  name: 'MPThemen',
  props: ['allCats'],
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
      return this.items.filter((x) => !x.closed).slice(0, 10)
    },
    cat () {
      return this.allCats[MP_THEMES_CAT_ID]
    },
    newCat () {
      return this.allCats[MP_THEMES_NEW_CAT_ID]
    },
    headerTitle () {
      return "Themenkreise"
    },
    kreise () {
      return this.cat.subcats.filter(x => x.id !== MP_THEMES_NEW_CAT_ID)
    },
    unseen () {
      return this.selected.filter(x => x.unseen)
    },
    catLink () {
      return `${MP_BASE_URL}/c/${this.cat.slug}`
    }
  },
  methods: {
    refreshItems () {
      fetch(`${MP_BASE_URL}/c/${this.cat.slug}/${this.newCat.slug}.json`, {
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
