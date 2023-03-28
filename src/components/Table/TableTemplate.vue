<script lang="ts">
import {defineComponent} from "vue";

import TableRow from "./TableRow.vue";
import IconSort from "../icons/IconSort.vue";

import type { PropType } from "vue";


interface Object {
  [key: string]: string | number | boolean
}

interface Objects extends Array<Object>{}
export default defineComponent ({
  components: {IconSort, TableRow},
  data(){
    return {
      pageNumber: 1 as number,
      rowsPerPage: 50 as number,
    }
  },
  props: {
    sortColumnData: {
      type: Function,
      required: false
    },
    deleteRow: {
      type: Function,
      required: false
    },
    handleInput: Function,
    tableHeaders: Array,
    tableData: {
      type: Array as PropType<Objects>
    }
  },
  computed: {
    isAscendingObject(){
      return this.tableHeaders?.reduce((a: object, v: string) => ({ ...a, [v as string]: true}), {})
    },
    tableDataArray(){
      return JSON.parse(JSON.stringify(this.tableData));
    }
  },
  methods:{
    sortData(header: string, isAscending = true){
      this.sortColumnData !== undefined && this.sortColumnData(header, isAscending);
      (this.isAscendingObject as Object)[header] = !isAscending;
    },
    onInput(event: InputEvent){
      this.handleInput !== undefined && this.handleInput(event)
    },
  },
})
</script>
<template>
  <div class="table-container">
    <table class="table">
      <thead class="table__heading">
      <tr class="table__headers">
        <th class="header table__header" v-for="(header, index) in this.tableHeaders" :key="index">
          <div class="header-container">
            {{header}}
            <button class="sort-button header-container__button" @click="this.sortData(header, this.isAscendingObject[header])">
              <IconSort class="sort-button__icon" />
            </button>
          </div>
        </th>
        <th class="header table__header" v-if="this.deleteRow || false">
          Actions
        </th>
      </tr>
      </thead>
      <tbody>
      <TableRow
          v-for="(row,index) in tableDataArray"
          :key="index"
          :row-data="row"
          :delete-row="this.deleteRow"
      />
      </tbody>
    </table>
  </div>
</template>
<style>
.table-container {
  overflow: auto;
}
.table {
  position: relative;
  width: 100%;
  border-collapse: collapse;
  table-layout:fixed
}
.table__heading {
}
.table__header {
  background: white;
  position: -webkit-sticky;
  position: sticky;
  top: 0;
  z-index: 2;
  padding: 16px 8px;
  font-weight: 700;
  font-size: 14px;
  line-height: 17px;
  text-transform: uppercase;
  /*box-shadow: 0 2px 2px -1px rgba(0, 0, 0, 0.4);*/
}

.table__header:first-child {
  padding: 16px 8px 16px 16px;
}

.table__header:last-child {
  padding: 16px 16px 16px 8px;
}

.header-container {
  display: flex;
  justify-content: space-between;
}

.header-container__button {
  width: 16px;
  height: 16px;
  border-style: none;
  padding: 0;
  color: #9E9E9E;
  background-color: transparent;
}

.sort-button__icon {
  width: 16px;
  height: 16px;
}

@media (min-width: 768px) {
  .table-container {
    overflow: unset;
  }

}
</style>