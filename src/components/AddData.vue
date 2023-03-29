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
  data(){
    return {
      isAllFieldsFilled: Boolean,
    }
  },
  computed: {
    dataFields(){
      return this.defaultDataFields?.filter((field: DataField) => field.isVisible);
    },
    addedData(){
      return this.defaultDataFields?.reduce((a: object, v: DataField) => ({ ...(a as object), [v.name]: ""}), {})
    },
  },
  methods: {
    onSubmit(event: Event){
      const isAllFieldsFilledArray = [];
      Object.values(this.addedData as Object).map((value) => {
        const isVisible = this.dataFields !== undefined &&
            this.dataFields.find((field: DataField) => field.isVisible);
        if(isVisible && value !== ""){
          isAllFieldsFilledArray.push(true);
        }
      })
      this.isAllFieldsFilled =
          isAllFieldsFilledArray.length === Object.keys(this.dataFields as DataFields).length;
      if(this.isAllFieldsFilled){
        this.addData !== undefined && this.addData(this.addedData);
        Object.keys(this.addedData as Object).forEach(key=> {
          (this.addedData as Object)[key as keyof Object] = "";
        })
        const target = event.target as HTMLFormElement;
        target?.reset();
      }
    }
  }
})

</script>
<template>
  <form @submit.prevent="onSubmit" class="add-data form--add-data">
    <label
        class="add-data__label"

        v-for="(field, index) in this.dataFields"
        :key="index"
        :for="field.name">
      {{field.name}}
      <input
          class="add-data__input"
          :id="field.name"
          :type="field.type"
          v-model.trim="this.addedData[field.name]"
      >
    </label>
    <div
        class="warning-message add-data__warning-message"
        :class="{'warning-message--hidden': this.isAllFieldsFilled}"
    >
      <span>Please fill all required fields</span>
    </div>
    <button class="add-data__button" type="submit">Submit</button>
  </form>
</template>
<style>
.add-data {
  display: flex;
  flex-direction: column;
  padding: 16px;
  margin-bottom: 16px;
  gap: 16px;
  max-width: 768px;
}
.add-data__button {
  display: flex;
  padding: 8px 12px;
  align-items: center;
  justify-content: center;
  color: #ffffff;
  border-radius: 8px;
  background: #624DE3;
  border: 1px solid #624DE3;
  font-weight: 500;
  font-size: 12px;
}
.add-data__button:hover{
  background-color: #F7F6FE;
  color: #624DE3;
}
.add-data__button:focus {
  color: #ffffff;
  background-color: rgba(98, 77, 227, 0.7);
  border: 1px solid rgba(98, 77, 227, 0.0);
}
.add-data__label {
  display: flex;
  flex-direction: column;
  text-transform: capitalize;
  gap: 8px;
  font-size: 14px;
  line-height: 17px;
}
.add-data__input {
  width: 100%;
  box-sizing: border-box;
  padding: 8px 9px;
  border: 1px solid #9E9E9E;
  border-radius: 8px;
}

.add-data__input:focus {
  outline: none ;
  border-color: #624DE3;
}

.add-data__input::placeholder{
  font-family: "Montserrat", sans-serif;
  font-style: normal;
  font-weight: 500;
  line-height: 15px;
  text-transform: capitalize;
  color: #9E9E9E;
}

.add-data__warning-message {
  font-size: 14px;
  line-height: 17px;
  color: #A30D11;
}
.warning-message--hidden {
  visibility: hidden;
}
</style>