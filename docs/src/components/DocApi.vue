<template lang="pug">
q-card.doc-api.q-my-lg(v-if="ready")
  q-toolbar.text-grey-8.bg-white
    card-title(:title="name", prefix="API--")
    .text-grey {{ type }}

  q-separator

  div.bg-grey-2.text-grey-7.flex.no-wrap
    q-tabs(v-model="currentTab", align="left", dense)
      q-tab(
        v-for="tab in tabs"
        :key="`api-tab-${tab}`"
        :name="tab"
        :label="tab"
      )

    q-input.col.q-mx-sm(
      ref="input",
      v-model="filter",
      dense,
      input-class="text-right",
      borderless,
      placeholder="Filter..."
    )
      q-icon.cursor-pointer(
        slot="append",
        :name="filter !== '' ? 'clear' : 'search'"
        @click="onFilterClick"
      )

  q-separator

  q-tab-panels(v-model="currentTab", animated)
    q-tab-panel(v-for="tab in tabs", :name="tab", :key="tab" class="q-pa-none")
      ApiRows(:which="tab", :api="filteredApi")
</template>

<script>
import ApiRows from './ApiRows.js'
import CardTitle from './CardTitle.vue'

export default {
  name: 'DocApi',

  components: {
    ApiRows,
    CardTitle
  },

  props: {
    file: {
      type: String,
      required: true
    }
  },

  data () {
    return {
      ready: false,
      currentTab: null,
      filter: '',
      filteredApi: {}
    }
  },

  watch: {
    filter (val) {
      val = val.trim().toLowerCase()

      if (val === '') {
        this.filteredApi = this.api
        return
      }

      const api = {}

      this.tabs.forEach(tab => {
        if (tab === 'injection') {
          api[tab] = this.api[tab]
          return
        }

        api[tab] = {}
        const tabApi = this.api[tab]

        Object.keys(tabApi).forEach(name => {
          if (
            (name.indexOf(val) > -1) ||
            (tabApi[name].desc !== void 0 && tabApi[name].desc.toLowerCase().indexOf(val) > -1)
          ) {
            api[tab][name] = tabApi[name]
          }
        })
      })

      this.filteredApi = api
    }
  },

  methods: {
    parseJson (name, { type, ...api }) {
      this.api = api
      this.filteredApi = api
      this.apiType = type

      this.name = name
      this.type = `${type === 'plugin' ? 'Quasar' : 'Vue'} ${type.charAt(0).toUpperCase()}${type.substring(1)}`
      this.tabs = Object.keys(api)
      this.currentTab = this.tabs[0]
    },

    onFilterClick () {
      if (this.filter !== '') {
        this.filter = ''
      }
      this.$refs.input.focus()
    }
  },

  mounted () {
    import(
      /* webpackChunkName: "quasar-api" */
      /* webpackMode: "lazy-once" */
      `quasar/dist/api/${this.file}.json`
    ).then(json => {
      this.parseJson(this.file, json.default)
      this.ready = true
    })
  }
}
</script>
