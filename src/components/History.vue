<template>
  <div>
    <h2 @click="select_history(null)">Page History</h2>
    <ul>
      <li v-for="hist in history"
          :class="hist_class(hist)"
          :key="hist.history_id"
          @click="select_history(hist)">
        {{hist.history_date}}
        <ul v-if="hist_selected(hist)">
          <li>{{hist.history_user__first_name}} {{hist.history_user__last_name}}</li>
        </ul>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'History',
  props: {
    history: Array,
    selected: String
  },
  methods: {
    select_history (hist) {
      this.$emit('select_history', this.history.indexOf(hist) ? hist : null)
    },
    hist_class (hist) {
      return { 'hist-selected': this.hist_selected(hist) }
    },
    hist_selected (hist) {
      return (this.selected === hist.history_id) ||
        (!this.selected && !this.history.indexOf(hist))
    }
  }
}
</script>

<style scoped>
.hist-selected {font-weight: bold}
</style>
