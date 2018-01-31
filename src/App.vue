<template>
  <div id="app">
    <span v-if="loading">
      loading
    </span>
    <b-container v-else fluid>
      <b-row>
        <b-col cols="8">
          <b-row>
            <b-card-group>
              <Plenum />
              <MPNotifications />
            </b-card-group>
          </b-row>
          <b-row>
            <b-card-group columns>
              <MPMine />
            </b-card-group>
          </b-row>
        </b-col>
        <b-col>
          <MPNews />
          <b-card>
            <iframe src="https://www.facebook.com/plugins/page.php?href=https%3A%2F%2Fwww.facebook.com%2Fdemokratiebewegen%2F&tabs=timeline&height=500&small_header=true&adapt_container_width=true&hide_cover=true&show_facepile=false" width="340" height="500" style="border:none;overflow:hidden" scrolling="no" frameborder="0" allowTransparency="true"></iframe>
          </b-card>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import { MP_BASE_URL } from './consts.js'
import Plenum from './components/Plenum'
import MPNews from './components/MPNews'
import MPMine from './components/MPMine'
import MPNotifications from './components/MPNotifications'

export default {
  name: 'App',
  components: {
    Plenum,
    MPNews,
    MPMine,
    MPNotifications
  },
  mounted () {
    this.refreshMPInfo()
  },
  data () {
    return { 'mp_info': [], 'loading': true }
  },
  computed: {
    my_categories () {
      return this.mp_info.categories.filter(x => x.notification_level > 1)
    }
  },
  methods: {
    refreshMPInfo () {
      fetch(MP_BASE_URL + '/site.json', {
        method: 'GET',
        mode: 'cors',
        credentials: 'include',
        cache: 'no-cache'
      }).then(x => x.json()
      ).then(data => { this.mp_info = data; this.loading = false }
      )
    }
  }
}
</script>
