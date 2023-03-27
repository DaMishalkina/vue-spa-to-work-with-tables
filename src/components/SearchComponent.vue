<script lang="ts">
import {defineComponent} from "vue";

import IconSearch from "./icons/IconSearch.vue";

export default defineComponent({
  components: {IconSearch},
  props:{
    id: String,
    defaultValue: String,
    handleInput: Function,
    defaultPlaceholder: String,
  },
  data (){
    return {
      value: this.defaultValue || "",
      placeholder: this.defaultPlaceholder || "search..."
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
    <input class="search-container__input"
           :placeholder="this.placeholder"
           :id=this.id
           type="text"
           v-model=this.value
           @input="this.onInput"
    >
    <IconSearch class="search-container__icon" />
  </label>
</template>
<style>
.search-container {
  position: relative;
  color: #9E9E9E;
}
.search-container__input {
  width: 100%;
  box-sizing: border-box;
  padding: 8px 9px 8px 33px;
  border: 1px solid #9E9E9E;
  border-radius: 8px;
}

.search-container__input:focus {
  outline: none ;
  border-color: #624DE3;
}

.search-container__input::placeholder{
  font-family: "Montserrat", sans-serif;
  font-style: normal;
  font-weight: 500;
  line-height: 15px;
  text-transform: capitalize;
  color: #9E9E9E;
}
.search-container__icon {
  position: absolute;
  width: 16px;
  height: 16px;
  top: 8px;
  left: 9px;
}
</style>