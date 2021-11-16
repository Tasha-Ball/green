<template>
  <div class="container">
    <div class="top">
      <h1>Таблица с комментариями</h1>
      <Pagination
        :sort="sort"
        :pages="pages"
        :pageLine="pageLine"
        :itemsLength="itemsLength"
        @pagination:next="onPagination"
        @pagination:prev="onPagination"
      />
    </div>
    <DataTable
      :sortable="true"
      :items="items"
      :headers="headers"
      :hovered="true"
      sortBy="name"
      sortType="desc"
      @on-sort="onFilter"
    />
  </div>
</template>

<script>
export default {
  data() {
    return {
      sort: false,
      pageLine: 10,
      pages: null,
      itemsLength: null,
      items: [],
      itemsAll: [],
      headers: [
        {text: 'ID', sortable: true, field: 'id', width: 50},
        {text: 'Имя', sortable: false, field: 'name', width: 250},
        {text: 'Электронная почта', sortable: false, field: 'email', width: 250}
      ]
    }
  },

  methods: {
    async fetch() {
      this.itemsAll = await fetch(
        `https://jsonplaceholder.typicode.com/comments`
      ).then(res => res.json())
      this.itemsLength = this.itemsAll.length
      this.pages = Math.ceil(this.itemsAll.length / this.pageLine)
      this.onFilter()
    },
    onFilter(params) {
      if (params && params.sortType) {
        let ind = null
        if (params.sortType === "asc") ind = -1
        if (params.sortType === "desc") ind = 1
        this.itemsAll.sort((a,b) => {
          if (a[params.sortBy] > b[params.sortBy]) return ind;
          if (a[params.sortBy] < b[params.sortBy]) return -ind;
          return 0;
        });
      }
      if (params && !params.sortType) {
        this.fetch()
      }
      this.items = this.itemsAll.slice(0, this.pageLine)
      this.sort = true
    },
    onPagination(limit) {
      this.sort = false
      this.items = this.itemsAll.slice(limit, this.pageLine+limit)
    },
  },
  created() {
    this.fetch()
  }
}
</script>

<style>
  .container {
    margin: 0 auto;
    position: relative;
    max-width: 100%;
    width: 1440px;
  }
  .top {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
</style>
