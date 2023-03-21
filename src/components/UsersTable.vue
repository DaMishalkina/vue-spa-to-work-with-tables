<script setup lang="ts">
import axios from "axios";
import {onMounted, ref, computed} from "vue";

import TableTemplate from "./Table/TableTemplate.vue";
import PaginationComponent from "./PaginationComponent.vue";

const TABLE_HEADERS = ["ID", "NAME", "EMAIL", "BODY" ];
const USERS_PER_PAGE = 50;

const users = ref([]);
let searchValues = ref({
  id: "" as string,
  name: "id" as string,
  body: "" as string,
});
const usersPerPage: number = USERS_PER_PAGE;
let currentPageNumber = ref(1);
const getUsers = async() => {
  try {
    const response = await axios.get("db.json");
    return response.data.users
  }
  catch (error){
    console.log(error)
  }
}
const paginatedUsers = computed(() => {
  let from = (currentPageNumber.value-1)*usersPerPage;
  let to = from + usersPerPage;
  const updatedData = JSON.parse(JSON.stringify(users.value));
  return updatedData.slice(from, to);
})

const search = () => {
  let result = [...users.value];
  Object.entries(searchValues.value).forEach((searchValue: string[]) => {
    const [key, value]: string[] = searchValue;
    const newValue = value.toString().toLowerCase();
    if (newValue !== ""){
      result = result.filter(user => {
        return (user[key] as string).toString().toLowerCase().includes(newValue);
      })
    }
  })

  return result;
};



const filteredItems = computed(() => {
  return search();
})

const isDataFiltered = computed(() => {
  const trueArray: boolean[] = [];
  Object.values(searchValues.value).forEach(value => {
    value.toString().length > 0 && trueArray.push(true)
  });

  return trueArray.length > 0;
})

const handlePageClick = (page: number) => {
  currentPageNumber.value = page;
}
onMounted(() => {
  getUsers().then(data => {
    users.value = data;
  });
})

</script>

<template>
  <main>
    <TableTemplate
        :table-data="isDataFiltered ? filteredItems : paginatedUsers"
        :table-headers="TABLE_HEADERS"
    />
    <PaginationComponent
        v-if="!isDataFiltered"
        :handle-page-click="handlePageClick"
        :default-items-per-page="usersPerPage"
        :default-page-number="currentPageNumber"
        :data="users"
    />
  </main>
</template>
