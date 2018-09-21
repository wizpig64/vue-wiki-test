<template>
  <div>
    <h2>Contributors</h2>
    <ul>
      <li v-for="user in users" :key="user.id">
        {{user.full_name}}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'Contributors',
  props: {
    users: Array
  }
}
</script>

<style scoped>

</style>
