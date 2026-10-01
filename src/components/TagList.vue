<template>
  <ul class="tag-list">
    <li class="tag-default tag-pill tag-outline" v-for="tag of tags" :key="tag">
      <span v-text="tag" />
    </li>
  </ul>
</template>

<script setup>
defineProps({
  tags: Array,
  default: () => []
});
</script>
