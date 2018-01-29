<template>
	<a v-bind:href="link">
		<span v-if="topic.closed">🔒</span>
		<span v-if="topic.unseen" class="text-bold" >{{topic.fancy_title}}</span>
		<span v-else>{{topic.fancy_title}}</span>
	</a>
</template>

<script>

import {MP_BASE_URL} from '../consts.js'

export default {
  name: 'MPTopicLink',
  props: ['topic'],
  computed: {
    link () {
      return MP_BASE_URL + '/t/' + this.topic.slug + '/' + this.topic.id + '/'
    }
  }
}

</script>