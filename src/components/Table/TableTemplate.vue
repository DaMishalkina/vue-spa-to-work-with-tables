<script lang="ts">
import {defineComponent} from "vue";

import TableRow from "./TableRow.vue";
import SearchComponent from "../SearchComponent.vue";

import type { PropType } from "vue";

interface Object {
  [key: string]: string | number
}

interface Objects extends Array<Object>{}
export default defineComponent ({
  components: {TableRow, SearchComponent},
  data(){
    return {
      pageNumber: 1 as number,
      rowsPerPage: 50 as number,
    }
  },
  props: {
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
      type: Array as PropType<Objects>,
      default: () =>  []
    }
  },
  computed: {
    tableDataArray(){
      return JSON.parse(JSON.stringify(this.tableData));
    }
  },
  methods:{
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