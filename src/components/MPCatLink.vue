<template>
  <a :href="link" :class="klasses" :style="style">
    {{cat.name}}
  </a>
</template>

<script>

import {MP_BASE_URL} from '../consts.js'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'

export default {
  name: 'MPCatLink',
  props: ['cat', 'badge'],
  components: {
    FontAwesomeIcon
  },
  computed: {
    klasses () {
      if (this.badge) {
        return 'btn btn-sm btn-outline-light m-1'
      }
    },
    style () {
      if (this.badge) {
        return `border-color:#${this.cat.color}; color:#${this.cat.color}; font-size:0.8em`
      }
    },
    link () {
      return MP_BASE_URL + '/c/' + this.cat.parent.slug + '/' + this.cat.slug + '/'
    }
  }
}

</script>
