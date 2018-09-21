<template>
  <ol class="breadcrumb mt-3">
    <li class="breadcrumb-item"
        v-for="parent in parents"
        :key="parent">
      <a :href="parent">{{last_bit(parent)}}</a>
    </li>
    <li class="breadcrumb-item active">{{last_bit(path)}}</li>
  </ol>
</template>

<script>
export default {
  name: 'Breadcrumbs',
  props: ['parents', 'path'],
  methods: {
    last_bit (path) {
      let bits = path.split('/').filter(bit => bit !== '')
      return bits.length ? bits[bits.length - 1] : 'wiki'
    }
  }
}
</script>
