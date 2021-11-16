<template>
    <div class="pagination">
      <button @click="onPrev"
        :disabled="page === 1"
        :class="{'page': page !== 1}"
      >
        <i aria-hidden="true">&lt;</i>
      </button>
      <span>{{page + " из " + pages}}</span>
      <button @click="onNext"
        :disabled="page === pages"
        :class="{'page': page !== pages}"
      >
        <i aria-hidden="true">&gt;</i>
      </button>
    </div>
</template>

<script>
export default {
  data() {
    return {
      page: 1,
      limit: 0
    }
  },
  props: {
    sort: {
      type: Boolean,
      default: false,
    },
    pages: {
      type: Number,
      default: 1,
    },
    pageLine: {
      type: Number,
      default: 10,
    },
    itemsLength: {
      type: Number,
      default: 1,
    },
  },
  methods: {
    onPrev() {
      this.page -=1;
      if ( this.page === 0 ) {
        this.page = 1;
      }
      this.limit = this.limit-this.pageLine===0 ? 0 : this.limit-this.pageLine
      this.$emit("pagination:next", this.limit);
    },
    onNext() {
      this.page +=1;
      if ( this.page >= this.pages) {
        this.page = this.pages;
      }
      this.limit = this.limit+this.pageLine>=this.itemsLength ? this.limit : this.limit+this.pageLine
      this.$emit("pagination:prev", this.limit );
    }
  },
  watch: {
    sort: {
      handler: function() {
        if (this.sort) {
          this.page = 1
          this.limit = 0
        }
      }
    }
  }
}
</script>

<style>
  .pagination {
    display: flex;
    align-items: flex-end;
    min-width: auto;
    height: 30px;
    margin: 20px 0px;
  }
  .pagination.no-pages {
    box-shadow: none;
    pointer-events: none;
  }
  .pagination span {
    margin: 0px 10px;
    font-family: Ubuntu;
    font-style: normal;
    font-weight: normal;
    font-size: 16px;
    line-height: 21px;
  }
  .pagination button {
    background: transparent;
    border: none;
    border-radius: 50%;
    min-width: 30px;
    height: 30px;
    align-items: center;
    display: inline-flex;
    flex: 0 0 auto;
    justify-content: center;
    outline: 0;
    text-transform: uppercase;
    text-decoration: none;
    -webkit-transition: 0.3s cubic-bezier(0.25, 0.8, 0.5, 1), color 1ms;
    transition: 0.3s cubic-bezier(0.25, 0.8, 0.5, 1), color 1ms;
    position: relative;
    vertical-align: middle;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
  }
  .pagination .page:hover, .page:focus {
    background: #edfdfb;
    color: #0E655B;
    font-weight: bold;
    cursor: pointer;
  }

  .pagination button i {
    font-weight: 400;
    font-style: normal;
    font-size: 24px;
    line-height: 1;
    letter-spacing: normal;
    text-transform: none;
    display: inline-block;
    white-space: nowrap;
    overflow-wrap: normal;
    direction: ltr;
    font-feature-settings: "liga";
    -webkit-font-smoothing: antialiased;
    font-family: "Material Icons", sans-serif !important;
  }
</style>
