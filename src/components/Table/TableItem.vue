<script setup lang="ts">
defineProps<{
  value?: number | string;
}>()
</script>
<template>
  <td class="table__column">{{ value }}</td>
</template>
<style>
.table__column {
  padding: 16px 8px;
  font-size: 14px;
  line-height: 17px;
  max-width: 300px;
  margin: 0 auto;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
.table__column:first-child {
  padding: 16px 8px 16px 16px;
}

.table__column:last-child {
  padding: 16px 16px 16px 8px;
}
</style>