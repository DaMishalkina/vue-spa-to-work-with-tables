<script lang="ts">
import {defineComponent} from "vue";

import type {PropType} from "vue";

const PREV = "Previous";
const NEXT = "Next";

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
      previous: PREV,
      next: NEXT,
      itemsPerPage: this.defaultItemsPerPage || 10,
    }
  },
  computed: {
    currentPageNumber(){
      return this.defaultPageNumber || 1
    },
    arrayToDevelop(){
      return JSON.parse(JSON.stringify(this.data));
    },
    pages() {
      return Math.ceil(this.arrayToDevelop.length/this.itemsPerPage);
    },
    visiblePages() {
      const prev = this.previous;
      const next = this.next;
      let result;
      const gap = "...";
      const start = 1;
      const end = this.pages;
      const resultArr = [prev, start, end, next];
      if(this.pages > 5){
        switch (this.currentPageNumber){
          case start:
            resultArr.splice(2, 0, gap);
            break;
          case end:
            resultArr.splice(2, 0, gap);
            break;
          case start + 1:
            resultArr.splice(2, 0, this.currentPageNumber, gap);
            break;
          case end - 1:
            resultArr.splice(2, 0, gap, this.currentPageNumber);
            break;
          default:
            resultArr.splice(2, 0, gap, this.currentPageNumber, gap);
        }
        result = resultArr;
      } else {
        result = [prev, ...Array.from({length: this.pages}, (_, i) => i + 1) , next];
      }
      return result;
    }
  },
  methods: {
    turnPage(page: number, id = ""){
      if(this.handlePageClick !== undefined)
        switch (id){
          case PREV:
            this.handlePageClick(page - 1);
            break;
          case NEXT:
            this.handlePageClick(page + 1);
            break;
          default:
            this.handlePageClick(page);
        }
    },
  }
})
</script>
<template>
  <ul class="pagination">
    <li class="pagination__item"
        :class="{'page-control': page === this.previous || page === this.next,
        'page-number': typeof page === 'number'}"
         v-for="page in visiblePages"
         :key="page"
    >
      <button
          v-if="typeof page === 'number'|| page === this.previous || page === this.next"
          @click="typeof page === 'number' ? turnPage(page) : turnPage(this.currentPageNumber, page)"
          :class="{'page-number__button--active': page === this.currentPageNumber,
          'page-control__button': page === this.previous || page === this.next,
          'page-number__button': typeof page === 'number'}"
          :disabled="(page === this.previous && this.currentPageNumber === 1)
          || (this.currentPageNumber === this.pages && page === this.next)
          || this.currentPageNumber === page"
      >
        {{page}}
      </button>
      <span v-else>{{page}}</span>
    </li>
  </ul>
</template>
<style>
.pagination {
  list-style: none;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  gap: 12px;
  margin: 16px 0;
  padding: 0;
}
.page-control__button {
  border-style: none;
  background-color: transparent;
  font-weight: 500;
  font-size: 12px;
  line-height: 15px;
}

.page-control__button:disabled{
  color: #9E9E9E;
  cursor: default;
}
.page-control__button:hover:enabled {
  color: #624DE3;
}
.page-number__button {
  border-radius: 8px;
  border: 1px solid #E0E0E0;
  background: #E0E0E0;
  padding: 8px 9px;
  font-size: 12px;
  line-height: 15px;
  min-width: 31px;
}
.page-number__button--active {
  color: #ffffff;
  border-radius: 8px;
  background: #624DE3;
  border: 1px solid #624DE3;
}
.page-number__button:hover {
  background-color: #F7F6FE;
  color: #624DE3;
  border-color: #624DE3;
}

.page-number__button:focus {
  color: #ffffff;
  background-color: rgba(98, 77, 227, 0.7);
  border: 1px solid rgba(98, 77, 227, 0.0);
}

</style>