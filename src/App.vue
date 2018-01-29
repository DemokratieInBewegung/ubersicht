<template>
  <div id="app">
    <b-container fluid>
      <b-card-group columns>
        <Plenum />
        <MPNews />
        <MPNotifications />
        <b-card>
          <iframe src="https://www.facebook.com/plugins/page.php?href=https%3A%2F%2Fwww.facebook.com%2Fdemokratiebewegen%2F&tabs=timeline&height=500&small_header=true&adapt_container_width=true&hide_cover=true&show_facepile=false" width="340" height="500" style="border:none;overflow:hidden" scrolling="no" frameborder="0" allowTransparency="true"></iframe>
        </b-card>
      </b-card-group>
    </b-container>
  </div>
</template>

<script>
import Plenum from './components/Plenum'
import MPNews from './components/MPNews'
import MPNotifications from './components/MPNotifications'

export default {
  name: 'App',
  components: {
    Plenum,
    MPNews,
    MPNotifications
  }
}
</script>
