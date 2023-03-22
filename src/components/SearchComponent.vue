<script lang="ts">
import {defineComponent} from "vue";

import IconSearch from "./icons/IconSearch.vue";

export default defineComponent({
  components: {IconSearch},
  props:{
    id: String,
    defaultType: String,
    defaultValue: String,
    handleInput: Function,
  },
  data (){
    return {
      value: this.defaultValue || "",
      type: this.defaultType || "text"
    }
  },
  methods: {
    onInput(event: InputEvent){
      this.handleInput !== undefined && this.handleInput(event)
    }
  },
  computed: {

  },
  mounted() {
  }
})
</script>
<template>
  <label :for=this.id class="search-container">
    <input class="search-container__input" :id=this.id :type=this.type v-model=this.value @input="this.onInput">
    <IconSearch class="search-container__icon" />
  </label>
</template>
<style>
.search-container {
  position: relative;
}
.search-container__input {
}
.search-container__icon {
  position: absolute;
  width: 20%;
  height: 100%;
  right: 0;
  top: 0;
}
</style>