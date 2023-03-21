<script lang="ts">
import {defineComponent} from "vue";

import TableItem from "./TableItem.vue";

import type { PropType } from "vue";
interface AnyObject {
  [key: string]: string | number;
}
export default defineComponent({
  components: {TableItem},
  props: {
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
    }
  }
})
</script>

<template>
  <tr>
    <TableItem v-for="(value, index) in rowDataValues" :key=index :value="value"/>
  </tr>
</template>
<style></style>