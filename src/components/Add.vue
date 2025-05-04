<script setup>
import { nanoid } from "nanoid";
import { ref } from "vue";
const emit = defineEmits(["update-view", "add-item"]);

const form = ref({
  text: "",
});

const addItem = () => {
  emit("add-item", { id: nanoid(), event: form.value.text.trim() });
  console.log("已新增");
};
</script>

<template>
  <h2>正在新增</h2>

  <form>
    <input type="text" v-model="form.text" />
    <button type="button" @click="addItem">送出</button>
  </form>
</template>
