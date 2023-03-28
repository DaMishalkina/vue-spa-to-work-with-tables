<script>
import {defineComponent} from "vue";

export default defineComponent({
  props: {
    onClick: Function,
    text: String,
    disabled: {
      type: Boolean,
      required: false,
    }
  },

})
</script>
<template>
  <button
      :disabled="this.disabled"
      @click="onClick"
      class="custom-button">
    {{text}}
  </button>
</template>
<style scoped>
@import "./CustomButtonStyle.css";
</style>