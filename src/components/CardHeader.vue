<template>
  <span>
    <a :href="link" v-if="link">{{title}}</a>
    <span v-else>{{title}}</span>
    <font-awesome-icon v-if="icon" v-bind:class="iconClass" :icon="icon"/>
    <b-badge v-if="badge" pill :variant="badge-variant">{{badge}}</b-badge>
</span>
</template>

<script>

import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'

export default {
  name: 'CardHeader',
  props: ['title', 'link', 'icon', 'iconClass', 'badge', 'badge-variant'],
  components: {
    FontAwesomeIcon
  }
}

</script>
