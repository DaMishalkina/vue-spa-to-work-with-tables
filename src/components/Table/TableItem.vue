<script setup lang="ts">
defineProps<{
  value?: number | string;
}>()
</script>
<template>
  <td class="table__column">{{ value }}</td>
</template>
<style></style>