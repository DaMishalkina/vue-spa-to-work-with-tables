<script setup lang="ts">
import UsersTable from "../components/UsersTable.vue";
</script>

<template>
  <main class="page">
    <UsersTable />
  </main>
</template>
<style>
.page {
  display: flex;
  flex-direction: column;
}
@media (min-width: 1024px) {
  .page {
    margin: 0 auto;
    max-width: 1024px;
  }
}
</style>
