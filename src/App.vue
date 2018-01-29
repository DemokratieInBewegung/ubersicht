<template>
  <div id="app">
    <b-container fluid>
      <b-card-group columns>
        <Plenum />
        <MPNews />
        <MPNotifications />
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
