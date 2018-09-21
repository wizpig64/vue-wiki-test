<template>
  <b-container>
    <breadcrumbs :parents="path_parents" :path="path_clean"/>
    <div v-if="error" class="alert alert-danger" role="alert">
      Error sending request to server. You're probably not logged in.
    </div>
    <div class="row" v-if="resource">
      <div class="col-md-3">
        <contributors :users="page.contributors"/>
        <history :history="resource.history"
                 :selected="selected_history_id"
                 @select_history="select_history"/>
      </div>
      <div class="col-md-9">
        <editor :document="current_document"
                :history="selected_history"
                @select_history="select_history"
                @revert="revert"
                @save="save"/>
      </div>
    </div>
    <div v-else>
      <p>
        Could not find wiki page -- would you like to create on here at
        <code>{{path_clean}}</code>?
      </p>
      <p>
        <button type="button" class="btn btn-sm btn-default" @click="create">
          Create Page
        </button>
      </p>
    </div>
  </b-container>
</template>

<script>
import Breadcrumbs from './components/Breadcrumbs'
import Contributors from './components/Contributors'
import History from './components/History'
import Editor from './components/Editor'
import json_data from './sample.json'

export default {
  name: 'App',
  components: {
    Breadcrumbs,
    Contributors,
    History,
    Editor
  },
  data () {
    return {
      error: null,
      page: null, // this is set once in created(), includes initial resource
      path_clean: null,
      path_parents: [],
      resource: null,
      selected_history: null
    }
  },
  methods: {
    select_history (hist) {
      this.selected_history = hist
    },
    revert (history_id) {
      history_id
      // send the server the signal to roll back the page
      // axios.post(this.page.resource_api_url + 'revert/', { history_id })
      //   .then(response => {
      //     // response.data is a success message
      //     this.load_resource()
      //     this.unselect_history()
      //   })
      //   .catch(e => {
      //     this.error = e
      //   })
    },
    save (document) {
      document
      // push this document to the server as the latest version of the page
      // (api will silently handle no-change-changes for us.)
      // axios.put(this.page.resource_api_url, { document })
      //   .then(response => {
      //     this.resource = response.data
      //   })
      //   .catch(e => {
      //     this.error = e
      //   })
    },
    load_resource () {
      // axios.get(this.page.resource_api_url)
      //   .then((response) => {
      //     this.resource = response.data
      //   })
      //   .catch(e => {
      //     this.error = e
      //   })
    },
    create () {
      // create a new page at this url, and then refresh the page to load the new stuff.
      // axios.post('/api/wiki/', { path: this.path_clean })
      //   .then(() => {
      //     location.reload()
      //   })
      //   .catch(e => {
      //     this.error = e
      //   })
    }
  },
  computed: {
    selected_history_id () {
      return this.selected_history ? this.selected_history.history_id : null
    },
    current_document () {
      return this.selected_history ? this.selected_history.document
        : this.resource.document
    }
  },
  created () {
    // get data from json encoded in the page
    this.page = json_data.page
    this.path_clean = json_data.path_clean
    this.path_parents = json_data.path_parents
    // we may have gotten here by typing in a sloppy path.
    // switch to the canonical path to fix this.
    history.replaceState(null, null, this.path_clean + window.location.search)
    // grab the initial version of resource nested within page
    this.resource = this.page.resource
  }
}
</script>

<style scoped>

</style>
