<script lang="ts">
import {defineComponent} from "vue";

import TableItem from "./TableItem.vue";
import IconDelete from "../icons/IconDelete.vue";

import type { PropType } from "vue";

interface AnyObject {
  [key: string]: string | number;
}
export default defineComponent({
  components: {IconDelete, TableItem},
  props: {
    deleteRow: {
      type: Function,
      required: false,
    },
    rowData: {
     type: Object as PropType<AnyObject>
   }
  },
  data (){
    return {
    }
  },
  computed: {
    rowDataValues() {
      return Object.values(this.rowData as AnyObject)
    },
  },
  methods: {
  }
})
</script>

<template>
  <tr class="table__row">
    <TableItem v-for="(value, index) in rowDataValues.splice(1)" :key=index :value="value"/>
    <td v-if="deleteRow" class="column table__column">
      <button
          class="column-button column__button"
          @click="this.deleteRow(this.rowData)"
      >
        <IconDelete class="column-button--delete" />
      </button>
    </td>
  </tr>
</template>
<style>
.column-button {
  padding: 12px;
}
.column-button--delete {
  height: 16px;
  width: 16px;
}
</style>