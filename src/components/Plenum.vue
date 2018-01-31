<template>
  <b-card header="Plenum">
    <Initiative v-bind:ini="item" v-for="item in items" v-bind:key="item.id" />
  </b-card>
</template>

<script>

const VOTING_BASE_URL = 'https://abstimmen.bewegung.jetzt'
const VOTING_URL = VOTING_BASE_URL + '/?f=d&f=v&f=a&f=r'

const Initiative = {
  props: ['ini'],
  template: '<a href="">{{ini}}</a>'
}

export default {
  name: 'Plenum',
  components: {
    Initiative
  },
  mounted () {
    this.refreshItems()
  },
  data () {
    return { 'items': [] }
  },
  methods: {
    refreshItems () {
      fetch(VOTING_URL, {
        method: 'GET',
        mode: 'cors',
        credentials: 'include',
        cache: 'no-cache',
        headers: new Headers({
          // 'Accept': 'application/json',
          'X-Requested-With': 'XMLHttpRequest'
        })}
      ).then(x => x.content.initiatives
      ).then(data => { this.items = data }
      )
    }
  }
}
</script>
