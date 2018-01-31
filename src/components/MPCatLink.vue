<template>
  <a :href="link">
    {{cat.name}}
  </a>
</template>

<script>

import {MP_BASE_URL} from '../consts.js'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'

export default {
  name: 'MPCatLink',
  props: ['cat', 'parent'],
  components: {
    FontAwesomeIcon
  },
  computed: {
    link () {
      return MP_BASE_URL + '/c/' + this.cat.parent.slug + '/' + this.cat.slug + '/'
    }
  }
}

</script>
