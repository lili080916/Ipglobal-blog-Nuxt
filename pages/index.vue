<template>
  <div>
    <div class="posts-list">

      <TablePost/>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PostPage',
  layout: 'header',
  data() {
    return {}
  },

  methods: {

  }
}
</script>

<style lang="scss" scoped>
    .posts-list {
        position: relative;
    }
</style>
