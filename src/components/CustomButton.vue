<script lang="ts">
import {defineComponent} from "vue";

export default  defineComponent({
  props: {
    type: {
      type: String,
      required: false,
    },
    onClick: Function,
    isDisabled: {
      type: Boolean,
      required: false
    },
    class: {
      type: String,
      required: false
    },
    children: {
      type: Object,
      required: false,
    }
  }
})
</script>
<template>
  <button
      class="custom-button"
      :class="this.class"
      :type="this.type"
      :disabled="this.isDisabled"
      @click="this.onClick"
  >
    <slot class="custom-button__container" />
  </button>
</template>
<style>
.custom-button {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 8px 12px;
  color: #ffffff;
  border-radius: 8px;
  background: #624DE3;
  border: 1px solid #624DE3;
  font-weight: 500;
  font-size: 12px;
}
.custom-button:hover{
  background-color: #F7F6FE;
  color: #624DE3;
}
.custom-button:focus {
  color: #ffffff;
  background-color: rgba(98, 77, 227, 0.7);
  border: 1px solid rgba(98, 77, 227, 0.0);
}

</style>