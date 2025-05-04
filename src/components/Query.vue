<script setup>
const emit = defineEmits(["update-view"]);

const ReturnIndex = () => {
    emit("update-view", "add")
};
</script>

<template>
  <h2>查詢11111111111</h2>

  <button @click="AddList">新增</button>
</template>
