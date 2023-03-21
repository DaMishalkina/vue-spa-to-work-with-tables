<script lang="ts">
import {defineComponent} from "vue";

import TableRow from "./TableRow.vue";

import type { PropType } from "vue";

interface Object {
  [key: string]: string | number
}

interface Objects extends Array<Object>{}
export default defineComponent ({
  components: {TableRow},
  data(){
    return {
      pageNumber: 1 as number,
      rowsPerPage: 50 as number,
    }
  },
  props: {
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
  }
})
</script>
<template>
  <table>
    <thead>
      <tr>
        <th v-for="(header, index) in this.tableHeaders" :key="index">{{header}}</th>
      </tr>
    </thead>
    <tbody>
    <TableRow v-for="(row,index) in tableDataArray" :key="index" :row-data="row"/>
    </tbody>
  </table>
</template>
<style>
</style>