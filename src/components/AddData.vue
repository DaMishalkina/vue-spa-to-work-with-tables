<script lang="ts">

import {defineComponent} from "vue";

import type { PropType } from "vue";

interface DataField {
  name: string,
  type: string,
  isVisible: boolean,
}

interface Object {
  [key: string]: string | number | boolean
}

interface DataFields extends Array<DataField>{}

export default defineComponent({
  props: {
    addData: {
      type: Function
    },
    defaultDataFields: Array as PropType<DataFields>
  },
  data() {
    return {
    }
  },
  computed: {
    dataFields(){
      return this.defaultDataFields?.filter(field => field.isVisible);
    },
    addedData(){
      return this.defaultDataFields?.reduce((a, v) => ({ ...(a as object), [v.name]: ""}), {})
    }
  },
  methods: {
    onSubmit(event: SubmitEvent){
      const isAllFieldsFilledArray = [];
      Object.values(this.addedData as Object).map((value) => {
        const isVisible = this.dataFields !== undefined && this.dataFields.find(field => field.isVisible);
        if(isVisible && value !== ""){
          isAllFieldsFilledArray.push(true);
        }
      })
      const isAllFieldsFilled = isAllFieldsFilledArray.length === Object.keys(this.dataFields as DataFields).length;
      if(isAllFieldsFilled){
        this.addData !== undefined && this.addData(this.addedData);
        Object.keys(this.addedData as Object).forEach(key=> {
          (this.addedData as Object)[key as keyof Object] = "";
        })
        const target = event.target as HTMLFormElement;
        target?.reset();
      } else {
        alert("Please fill all required fields")
      }
    }
  },
  mounted() {
  }
})

</script>
<template>
  <form @submit.prevent="onSubmit">
    <label v-for="(field, index) in this.dataFields" :key="index" :for="field.name">
      {{field.name}}
      <input
          :id="field.name"
          :type="field.type"
          v-model.trim="this.addedData[field.name]"
      >
    </label>
    <button type="submit">Submit</button>
  </form>
</template>
<style></style>