<script setup lang="ts">
defineProps<{
  value?: number | string;
}>()
</script>
<template>
  <td>{{ value }}</td>
</template>
<style></style>