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
    }
  },
  props: {
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
  methods: {

  }
})
</script>
<template>
  <table>
    <thead>
      <tr>
        <td>postId</td>
        <td>id</td>
        <td>name</td>
        <td>email</td>
        <td>body</td>
      </tr>
    </thead>
    <tbody>
    <TableRow v-for="(row,index) in tableDataArray" :key="index" :row-data="row"/>
    </tbody>
  </table>
</template>
<style></style>