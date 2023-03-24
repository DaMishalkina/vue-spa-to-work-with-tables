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
      return this.tableHeaders?.reduce((a, v) => ({ ...(a as object), [v as string]: true}), {})
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
  <table class="table">
    <thead class="table__heading">
      <tr class="table__headers">
        <th class="header table__header" v-for="(header, index) in this.tableHeaders" :key="index">
          <div class="header-container">
            <div></div>
            {{header}}
            <button class="header-container__button button" @click="this.sortData(header, this.isAscendingObject[header])">
              <IconSort />
            </button>
          </div>
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
</template>
<style>
.header-container {
  display: flex;
}
</style>