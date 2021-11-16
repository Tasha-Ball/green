<template>
  <DetailComment
    :items="row"
  />
</template>

<script>
export default {
  name: 'comment',
  data() {
    return {
      row: {}
    }
  },
  mounted() {
    this.row = this.$route.params.row
  }
}
</script>

<style>

</style>
