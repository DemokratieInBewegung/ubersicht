<template>
  <span>
    {{title}}
    <font-awesome-icon v-if="icon" v-bind:class="iconClass" :icon="icon"/>
    <b-badge v-if="badge" pill :variant="badge-variant">{{badge}}</b-badge>
</span>
</template>

<script>

import {MP_BASE_URL} from '../consts.js'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'

export default {
  name: 'CardHeader',
  props: ['title', 'icon', 'iconClass', 'badge', 'badge-variant'],
  components: {
    FontAwesomeIcon
  }
}

</script>
