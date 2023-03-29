<script setup lang="ts">
import axios from "axios";
import {computed, onMounted, ref} from "vue";

import TableTemplate from "./Table/TableTemplate.vue";
import PaginationComponent from "./PaginationComponent.vue";
import AddData from "./AddData.vue";
import SearchComponent from "./SearchComponent.vue";
import IconPlus from "./icons/IconPlus.vue";
import CustomButton from "@/components/CustomButton.vue";

const TABLE_HEADERS = ["ID", "NAME", "EMAIL", "BODY" ];
const USERS_PER_PAGE = 50;

type User = {
  postId: string | number,
  id: string | number,
  name: string,
  email: string,
  body: string
}

const users = ref([] as User[]);
const usersArray = ref([] as User[]);

interface DataField {
  name: string,
  type: string,
  isVisible: boolean,
}
interface DataFields extends Array<DataField>{}
let searchValue = ref("");
const usersPerPage: number = USERS_PER_PAGE;
let currentPageNumber = ref(1);
let isUserFormVisible = ref(false);
const getUsers = async() => {
  try {
    const response = await axios.get(process.env.NODE_ENV === "production"
        ? "https://raw.githubusercontent.com/DaMishalkina/vue-spa-to-work-with-tables/main/db.json"
        : "db.json");
    return response.data.users
  }
  catch (error){
    console.log(error)
  }
}
const paginatedUsers = computed(() => {
  let from = (currentPageNumber.value-1)*usersPerPage;
  let to = from + usersPerPage;
  const updatedData = JSON.parse(JSON.stringify(usersArray.value));
  return updatedData.slice(from, to);
})

const search = () => {
  let result = [...usersArray.value];
  const newSearchValue = searchValue.value.toString().toLowerCase();
  if (newSearchValue !== ""){
    result = result.filter(user => {
      return Object.values(user).join("").toString().toLowerCase().includes(newSearchValue)
    })
  }

  return result;
};



const filteredItems = computed(() => {
  return search();
})
const dataFields = computed(() => {
  const result: DataFields = [];
 Object.entries(usersArray.value[0] || {}).map(entry => {
   const [key, value] = entry;
   const object = {
     name: key,
     type: typeof value,
     isVisible: key !== "id" && key !== "postId"
   }
   result.push(object);
 })
  return result;
})

const isDataFiltered = computed(() => {
  return searchValue.value.length > 0;
})

const handlePageClick = (page: number) => {
  currentPageNumber.value = page;
}

const compare = (a: string | number, b: string | number) => {
  if(typeof a === "string"){
    return a.localeCompare(b as string)
  } else {
    return a - (b as number)
  }
}

const sortData = (header: string, isAscending = true) => {
  const lowerCaseHeader = header.toLowerCase();
  switch (isAscending){
    case true:
      usersArray.value.sort((a, b) =>
          compare(a[lowerCaseHeader as keyof User], b[lowerCaseHeader as keyof User]));
      break;
    case false:
      usersArray.value.sort((a, b) =>
          compare(b[lowerCaseHeader as keyof User], a[lowerCaseHeader as keyof User]));
  }
}

const deleteItem = (objectToDelete: User) =>{
  usersArray.value = usersArray.value.filter(user => (user as User).id !== objectToDelete.id)
}

const handleInput = (event: InputEvent) => {
  searchValue.value = (event?.target as HTMLInputElement)?.value;
}

const isPaginated = computed(() => {
  return Math.ceil(usersArray.value.length/usersPerPage) > 1;
})

const handleSubmit = (data: User) => {
  const arrToSort = [...usersArray.value];
  const ascendingSortedArr = arrToSort.sort((a,b) => compare(a.id, b.id));
  data.id = (ascendingSortedArr[ascendingSortedArr.length -1].id as number) + 1;
  usersArray.value.push({...data});
}
const toggleNewUserForm = () => {
  isUserFormVisible.value = !isUserFormVisible.value;
}
onMounted(() => {
  getUsers().then(data => {
    users.value = data;
    usersArray.value = [...users.value];
  });
})

</script>

<template>
  <section class="section-header">
    <SearchComponent
        :handle-input="handleInput"
        :default-value="searchValue"
    />
    <CustomButton :onClick="toggleNewUserForm"
    >
      <IconPlus class="icon--plus button__icon" />
      Add user
    </CustomButton>
  </section>
  <section class="section-main">
    <AddData v-if="isUserFormVisible" :add-data="handleSubmit" :default-data-fields="dataFields" />
    <TableTemplate
        :class="{
        'table-container--height90': isPaginated,
        'table-container--height100': !isPaginated }"
        :sort-column-data="sortData"
        :delete-row="deleteItem"
        :handle-input="handleInput"
        :table-data="isDataFiltered ? filteredItems : paginatedUsers"
        :table-headers="TABLE_HEADERS"
    />
    <PaginationComponent
        v-if="!isDataFiltered && isPaginated"
        :handle-page-click="handlePageClick"
        :default-items-per-page="usersPerPage"
        :default-page-number="currentPageNumber"
        :data="usersArray"
    />
  </section>
</template>
<style>
.section-header {
  display: flex;
  flex-direction: column;
  padding: 16px;
  justify-content: space-between;
  gap: 16px;
}

.button__icon {
  width: 16px;
  height: 16px;
  margin-right: 8px;
}

.section-main {
  height: 100vh;
}
.table-container--height90 {
  height: 90%;
}
.table-container--height100 {
  height: 100%;
}

@media (min-width: 768px) {
  .section-header {
    flex-direction: row;
  }
  .section-main {
    height: 100%;
  }
}

</style>
