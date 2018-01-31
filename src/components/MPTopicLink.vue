<template>
  <span>
    <font-awesome-icon class="text-secondary" icon="lock" v-if="topic.closed" />
    <a v-bind:href="link" v-bind:class="klasses">
    {{topic.fancy_title}}
    <span v-if="topic.unread" title="ungelese Antworten" class="badge badge-pill badge-primary">{{topic.unread}}</span>
  </a>
    <span v-if="topic.tags"><br />
      <span class="badge badge-light text-small text-info" v-for="tag in topic.tags" :key="tag">#{{tag}}</span>
    </span>
</span>
</template>

<script>

import {MP_BASE_URL} from '../consts.js'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'

export default {
  name: 'MPTopicLink',
  props: ['topic'],
  components: {
    FontAwesomeIcon
  },
  computed: {
    klasses () {
      if (this.topic.unseen) {
        return 'text-primary'
      }
      return 'text-secondary'
    },
    link () {
      return MP_BASE_URL + '/t/' + this.topic.slug + '/' + this.topic.id + '/'
    }
  }
}

</script>
