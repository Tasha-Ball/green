<template>
  <div>
      <div>
        <table class="table">
          <thead>
            <tr class="table-header-group">
              <template v-for="(header, index) of headers">
                <th
                  :key="index"
                  @click="header.sortable ? onSort($event, header.field) : null"
                  class="table-header"
                  :class="{
                    'p-20': !sortable,
                    'sortable': header.sortable && sortable,
                    'asc': sortBy === header.field && sortType=== 'asc' ? true : false,
                    'desc': sortBy === header.field && sortType=== 'desc' ? true : false
                  }"
                  :style="{ minWidth: `${header.width}px`, maxWidth: `${header.width}px`, width: `${header.width}px`}"
                >
                  {{header.text}}
                </th>
              </template>
            </tr>
          </thead>
        </table>
      </div>
      <div style="overflow: auto; transition: all 0.25s ease; max-width: 100%;">
        <table class="table" style="width: 100%;">
          <tbody :class="{hovered: hovered}" >
            <tr v-for="(row, index) of items"
              :key="index"
              @click.prevent.stop="onRowClick(row, $event)"
              :data-index="row.id"
              :class="{'row-active':row.$_active}"
            >
              <td v-for="(header, hI) of headers"
                :key="hI"
                :style="{
                  width: headers[hI].width+'px',
                  'min-width': headers[hI].width+'px',
                  'max-width': headers[hI].width+'px'
                }"
              >
                {{row[headers[hI].field]}}
              </td>
            </tr>
          </tbody>
        </table>
      </div>
  </div>
</template>

<script>
  export default {
    data() {
      return { }
    },
    props: {
      hovered: {
        type: [Boolean, String],
        default: false,
      },
      sortable: {
        type: [Boolean, String],
        default: false,
      },
      sortType: {
        type: String,
        default: null
      },
      sortBy: {
        type: String,
        default: null,
      },
      items: {
        type: Array,
        default: () => [],
      },
      headers: {
        type: Array,
        default: () => [],
      },
    },
    methods: {
      onSort({target}, field) {
        if ( !target.classList.contains("sortable") ) {
          return
        }

        const ths = target.parentNode.querySelectorAll("th");
        ths.forEach( (th) => {
          if (th !== target ) {
            th.classList.remove("asc")
            th.classList.remove("desc")
          }
        })

        if ( target.classList.contains("asc")) {
          target.classList.remove("asc")
          target.classList.add("desc")
          this.sBy = field
          this.sType = "desc"
        } else if ( target.classList.contains("desc")) {
          target.classList.remove("asc")
          target.classList.remove("desc")
          this.sBy = ""
          this.sType = ""
        } else {
          target.classList.add("asc")
          this.sBy = field
          this.sType = "asc"
        }
        this.$emit('on-sort', { sortBy: this.sBy, sortType: this.sType})
      },
      onRowClick(row, event) {
        if ( event.target.constructor.name === "HTMLTableCellElement" ) {
          this.$router.push({name: 'comment', params: {row: row, event: event}})
        }
      },
    }
  }
</script>

<style>
  .table {
    border-spacing: 0;
    border-collapse: collapse;
    width: 100%;
    border: 1px solid #EBEDF2;
    font-style: normal;
    font-weight: bold;
    font-size: 14px;
    line-height: 21px;
  }
  .table tbody.hovered tr:hover {
    background-color: #EBEDF2;
    transition: all 0.3s ease;
    cursor: pointer;
  }
  .table tbody.hovered tr.row-active{
    background-color: #EBEDF2;
    transition: all 0.3s ease;
    cursor: pointer;
  }
  .table tbody td span{
    pointer-events: none;
  }
  .table tbody td {
    max-width: 0px;
    border: 1px solid #EBEDF2;
    font-weight: normal;
    font-size: 12px;
    line-height: 21px;
    padding: 5px 20px;
    height: 34px;
    overflow: hidden;
    text-overflow: ellipsis;
    word-wrap: none;
    white-space: nowrap;
    box-sizing: border-box;
  }
  .table tbody td:first-child {
    text-align: center;
  }
  .table tbody tr.active {
    background-color: greenyellow !important;
  }
  .table tbody tr.disabled {
    background-color: hotpink !important;
  }
  .table tbody tr:nth-child(odd) {
    background-color: #fafafa
  }
  .table tbody tr:nth-child(even) {
    background-color: #fff
  }
  .table .table-header-group .table-header {
    cursor: pointer;
    position: relative;
    font-weight: bold;
    font-size: 14px;
    line-height: 16px;
    max-width: 0px;
    min-width: 45px;
    padding: 15px;
    box-sizing: border-box;
  }
  .table .table-header-group .table-header.sortable:after {
    display: block;
    position: absolute;
    content: "↑";
    width: 11px;
    height: 12px;
    font: normal normal normal 14px/1 'Material-Design-Iconic-Font';
    top: calc(50% - 5px);
    right: 3px;
    opacity: 1;
    transition: all 0.3s ease;
    color: #fff;
  }
  .table .table-header-group .table-header.sortable.asc:after {
    opacity: 1;
    transform: rotate(-180deg);
    transition: all 0.3s ease;
    color: #EFB42D;
  }
  .table .table-header-group .table-header.sortable.desc:after {
    opacity: 1;
    transition: all 0.3s ease;
    color: #EFB42D;
  }
  .table .table-header-group .table-header.border-bottom {
    border-bottom: 1px solid #EBEDF2;
  }
  .table .table-header-group .table-header .sortable {
    position: relative;
    display: block;
    width: 100%;
    margin: 5px 35px 0 0 ;
    padding-right: 5px;
  }
  .table thead .table-header-group th {
    background-color: rgba(53, 61, 86, 0.8) !important;
    vertical-align: middle;
    user-select: none;
    border: 1px solid #EBEDF2;
    border-bottom: 0;
    color: #fff;
    height: 25px;
  }
  .p-20 {
    padding: 20px !important;
  }
</style>
