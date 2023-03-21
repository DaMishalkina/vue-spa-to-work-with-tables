<script lang="ts">
import {defineComponent} from "vue";

import type {PropType} from "vue";

interface AnyObject {
  [key: string]: string | number;
}

interface Objects extends Array<AnyObject>{}
export default defineComponent({
  props: {
    defaultPageNumber: Number,
    defaultItemsPerPage: Number,
    data: Array as PropType<Objects>,
    handlePageClick: Function,
  },
  data (){
    return {
      currentPageNumber: this.defaultPageNumber || 1,
      itemsPerPage: this.defaultItemsPerPage || 10,
    }
  },
  computed: {
    arrayToDevelop(){
      return JSON.parse(JSON.stringify(this.data));
    },
    pages() {
      return Math.ceil(this.arrayToDevelop.length/this.itemsPerPage);
    },
  },
  methods: {
    turnPage(page: number){
      if(this.handlePageClick !== undefined)
      this.handlePageClick(page);
    },
  }
})
</script>
<template>
  <div class="v-table__pagintation">
    <div class="page"
         v-for="page in pages"
         :key="page"
         :class="{'page__selected': page === currentPageNumber}"
         @click="turnPage(page)">
      {{page}}
    </div>
  </div>
</template>
<style>
.v-table__pagintation {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  margin: 20px 0;
}
</style>