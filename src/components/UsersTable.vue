<script setup lang="ts">
import TableTemplate from "./Table/TableTemplate.vue"
import axios from "axios";
import {onMounted, ref} from "vue";

const users = ref([]);
const getUsers = async() => {
  try {
    const response = await axios.get("db.json");
    return response.data.users
  }
  catch (error){
    console.log(error)
  }

}
onMounted(() => {
  getUsers().then(data => {
    users.value = data;
  })
})

</script>

<template>
  <main>
    <TableTemplate :table-data=users />
  </main>
</template>
