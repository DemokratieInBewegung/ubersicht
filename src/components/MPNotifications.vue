<template>
  <b-card :header="header">
    <ul class="list-unstyled">
      <li v-for="notify in selected" :key="notify.id">
        <MPNofication v-bind:notification="notify" />
      </li>
      <li><a :href="moreLink"></a></li>
    </ul>
  </b-card>
</template>

<script>

import { MP_BASE_URL } from '../consts.js'
import MPTopicLink from './MPTopicLink'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'

const MP_URL = MP_BASE_URL + '/notifications.json?recent=true&limit=6'

const MPNofication = {
  name: 'MPNotification',
  props: ['notification'],
  components: {
    MPTopicLink,
    FontAwesomeIcon
  },
  computed: {
    moreLink () {
      return '' // FIXME links to self ... doesn't quite work yet
    },
    icon () {
      const typ = this.notification.notification_type
      if (typ === 6) {
        return 'envelope'
      } else if (typ === 5) {
        return 'heart'
      } else if (typ === 4) {
        return 'at'
      } else if (typ === 3) {
        return 'questionmark'
      } else if (typ === 2) {
        return 'questionmark'
      } else if (typ === 1) {
        return 'at'
      }
      return 'cogs'
    },
    inner_topic () {
      return {
        id: this.notification.topic_id,
        fancy_title: this.notification.fancy_title,
        slug: this.notification.slug,
        unseen: !this.notification.read
      }
    }
  },
  template: '<span><font-awesome-icon style="color: #ccc;" :icon="icon" /><MPTopicLink v-bind:topic="inner_topic" /></span>'
}

export default {
  name: 'MPNotifications',
  components: {
    MPNofication
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
        return 'Marktplatz Benachrichtigungen <b-badge>' + this.unseen.legth + '</b-bade>'
      }
      return 'Marktplatz Benachrichtigungen'
    }
  },
  methods: {
    refreshItems () {
      fetch(MP_URL, {
        // method: "GET",
        credentials: 'include'
        // headers: new Headers({
        //   'Accept': 'application/json',
        //   'X-Requested-With': 'XMLHttpRequest'
        // })
      }
      ).then(x => x.json()
      ).then(x => x.notifications
      ).then(data => { this.items = data; console.log(data) }
      )
    }
  }
}
</script>
