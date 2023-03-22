<script lang="ts">
import {defineComponent} from "vue";

export default defineComponent({
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
  <label :for=this.id>
    <input :id=this.id :type=this.type v-model=this.value @input="this.onInput">
  </label>
</template>
<style></style>