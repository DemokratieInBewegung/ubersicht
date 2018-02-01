<template>
  <div id="app">
    <span v-if="loading">
      loading
    </span>
    <b-container v-else fluid>
        <b-modal id="helpModal" header-text-variant="light" header-bg-variant="info" centered variant="secondary" size="lg" title="Hilfe">
          <div>
            <h3>Was ist das hier?</h3>
            <p>
              Dies ist Deine personalisierte DiB-Schaltzentrale. Hier laufen die Informationen aus all den Werkzeugen, die DiB verwendet zusammen und werden in einer für die persönlich zugeschnittene Fassung angezeigt
            </p>
            <h3>Was ist DiB?</h3>
            <p>
              DiB steht für DEMOKRATIE IN BEWEGUNG - eine junge neue progressive, werte-orientierte Partei. Du kannst mehr über DiB unter <a href="https://bewegung.jetzt/">bewegung.jetzt</a> erfahren.
            </p>
            <h3>Und das ist meine persönliche Zentrale dafür?</h3>
            <p>Genau. Hier laufen alle Informationen zu DiB für die zusammen. Damit Du immer den Überblick behälst - daher auch der name "ubersicht".</p>
            <h4>Heißt das es zieht Informationen als <strong>Ich</strong> aus dem Marktplatz?</h4>
            <p>Ja, im Prinzip schon. Wenn Du mit dem Browser auf dem Marktplatz und dem Plenum eingeloggt bist, zeigt es die Informationen für Dich personalisiert und zugeschnitten an -- inklusive der Information, ob DU es schon gelesen hast</p>
            <h4>Nice. Kannst du mir bissl mehr zu den Farben und Symbolen sagen?</h4>
            <p>Sicher. Jeder Block enthält die darin genannten Informationen. <span class="text-primary">Ungelese Beiträge sind dabei farbelich hervorgehoben</span> und die <b-badge pill variant="secondary">Zahl</b-badge> hinter dem Link zeigt an, wenn es neue Antworten dazu gibt - und wie viele.</p>
            <h4>Und die Blöcke hier in der Mitte?</h4>
            <p>Ganz in der Mitte zeigen wir die neuesten Inhalte aller Martkplatz-Kategorie, die du abonniert hast. Du kannst eine Kategorie abonnieren, in dem Du auf der Kategorie-Ansichs-Seite oben rechts auf den <b-button variant="secondary" size="sm"><font-awesome-icon :icon="['far', 'dot-circle']"/></b-button>-Button klickst. Wenn Du dort eine der oberen Drei ("Beobachten", "Verfolgen" oder "Ersten Beitrag beobachten") gewählt hast, wird die Kategorie beim Laden der Seite hier auch mit angezeigt.
            </p>
          </div>
          <div slot="modal-footer" class="w-100 text-right">
             <b-btn size="sm" variant="outline-info" href="mailto:support@bewegung.jetzt">
               Dem IT Support schreiben <font-awesome-icon :icon="['far', 'envelope']"/>
             </b-btn>
             <b-btn size="sm" variant="outline-primary" @click="show=false">
               Fenster schließen
             </b-btn>
            </div>
        </b-modal>
      <b-navbar toggleable="md" type="light" variant="faded">
        <b-navbar-toggle target="nav_collapse"></b-navbar-toggle>

        <b-navbar-brand><a href="https://bewegung.jetzt/">DiB</a>ubersicht</b-navbar-brand>
        <b-collapse is-nav id="nav_collapse">

          <b-navbar-nav>
            <b-nav-item href="https://abstimmen.bewegung.jetzt"><font-awesome-icon icon="tasks" /> Plenum</b-nav-item>
            <b-nav-item href="https://marktplatz.bewegung.jetzt"><font-awesome-icon icon="shopping-basket" /> Marktplatz</b-nav-item>
            <b-nav-item href="https://chat.bewegung.jetzt"><font-awesome-icon :icon="['far', 'comments']" /> Mattermost</b-nav-item>
            <b-nav-item href="https://wolke.bewegung.jetzt"><font-awesome-icon icon="cloud"/> Wolke</b-nav-item>
            <b-nav-item href="https://mail.bewegung.jetzt/SOGo"><font-awesome-icon :icon="['far', 'envelope']"/> E-Mail</b-nav-item>
            <b-nav-item v-b-modal.helpModal><font-awesome-icon :icon="['far', 'question-circle']"/> Hilfe</b-nav-item> 
          </b-navbar-nav>

          <!-- Right aligned nav items -->
          <b-navbar-nav class="ml-auto">
            <a v-if="session.username" :href="currentUserLink">
              <b-img  :src="currentUserAvatar" rounded="circle"  width="40" height="40" blank-color="#777" alt="Dein avatar auf dem Marktplatz" />
            </a>
            <b-img v-else rounded="circle" blank width="40" height="40" blank-color="#777" alt="Bitte melde dich auf dem Marktplatz an!" class="m-1" />
          </b-navbar-nav>

        </b-collapse>
      </b-navbar>
      <b-row>
        <b-col cols="8">
            <b-card-group class="my-3" deck>
              <Plenum />
              <MPNotifications />
            </b-card-group>
            <b-card-group columns>
              <MPMine />
              <MPThemen :allCats="all_categories" />
              <MPCategory v-for="c in my_categories" :cat="c" :allCats="all_categories" :key="c.id" />
            </b-card-group>
        </b-col>
        <b-col class="my-3">
          <MPMessages v-if="session" :user="session" />
          <MPCategory title="Marktplatz News" :cat="news_category" />
          <MPEvents />
          <b-card class="mt-3">
            <iframe src="https://www.facebook.com/plugins/page.php?href=https%3A%2F%2Fwww.facebook.com%2Fdemokratiebewegen%2F&tabs=timeline&height=500&small_header=true&adapt_container_width=true&hide_cover=true&show_facepile=false" width="340" height="500" style="border:none;overflow:hidden" scrolling="no" frameborder="0" allowTransparency="true"></iframe>
          </b-card>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import { MP_BASE_URL, MP_NEWS_CAT_ID } from './consts.js'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
import Plenum from './components/Plenum'
import MPCategory from './components/MPCategory'
import MPMine from './components/MPMine'
import MPThemen from './components/MPThemen'
import MPMessages from './components/MPMessages'
import MPEvents from './components/MPEvents'
import MPNotifications from './components/MPNotifications'

export default {
  name: 'App',
  components: {
    FontAwesomeIcon,
    Plenum,
    MPCategory,
    MPMine,
    MPThemen,
    MPMessages,
    MPEvents,
    MPNotifications
  },
  mounted () {
    this.refreshMPInfo()
  },
  data () {
    return { 'mp_info': [], 'all_categories': {}, 'loading': true, 'session': {} }
  },
  computed: {
    my_categories () {
      return this.mp_info.categories.filter(x => x.notification_level > 1)
    },
    news_category () {
      return this.mp_info.categories.find(x => x.id === MP_NEWS_CAT_ID)
    },
    currentUserAvatar () {
      return MP_BASE_URL + this.session.avatar_template.replace("{size}", "45")
    },
    currentUserLink () {
      return `${MP_BASE_URL}/u/${this.session.username}`
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
      ).then(mp_info => {
        const map = {}
        mp_info.categories.forEach((x) => {
          x.subcats = []
          map[x.id] = x 
        })
        mp_info.categories.forEach((x) => {
          if (x.parent_category_id) {
            map[x.parent_category_id].subcats.push(x)
            x.parent = map[x.parent_category_id]
          }
        })

        this.mp_info = mp_info
        this.all_categories = map
        this.loading = false

      })

      fetch(MP_BASE_URL + '/session/current.json', {
        method: 'GET',
        mode: 'cors',
        credentials: 'include',
        cache: 'no-cache'
      }).then(x => x.json()
      ).then(data => { this.session = data.current_user }
      )
    }
  }
}
</script>
