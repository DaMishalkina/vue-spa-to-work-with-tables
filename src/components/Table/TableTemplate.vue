<script lang="ts">
import {defineComponent} from "vue";

import TableRow from "./TableRow.vue";
import SearchComponent from "../SearchComponent.vue";
import IconSort from "../icons/IconSort.vue";

import type { PropType } from "vue";


interface Object {
  [key: string]: string | number | boolean
}

interface Objects extends Array<Object>{}
export default defineComponent ({
  components: {IconSort, TableRow, SearchComponent},
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
    searchValues: {
      type: Object as PropType<Object>,
      required: false,
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
    isFieldSearchable(index: string){
      return Object.keys(this.searchValues as Object).indexOf(index) !== -1;
    }
  },
})
</script>
<template>
  <table>
    <thead>
      <tr>
        <th v-for="(header, index) in this.tableHeaders" :key="index">
          <div>
            {{header}}
            <SearchComponent
                v-if="isFieldSearchable(header.toLowerCase())"
                :default-value="searchValues[header.toLowerCase()]"
                :id="header.toLowerCase()"
                :handle-input="onInput" />
            <button @click="this.sortData(header, this.isAscendingObject[header])">
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
</style>