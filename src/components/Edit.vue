<script setup>
import { nanoid } from "nanoid";
import { ref } from "vue";
const emit = defineEmits(["update-view", "add-item"]);

const props = defineProps({
  tagEdit: {
    type: Object,
  },
});

console.log("接收編輯",props.tagEditS);


const form = ref({
  text: props.tagEdit?.event ?? "",
});



// 🔹 ?. 是：「這孩子可能沒出生，就不要逼他叫名字」
// user?.name // 不報錯，如果 user 是 undefined，這會回傳 undefined


// 🔸 ?? 是：「如果沒寫就幫你補上」
// user.name ?? "預設名" // 如果是 undefined，就用 "預設名"


// 🔺 三元 ? : 是：「你自己判斷一下要用哪個」
// user.name ? user.name : "匿名"



const editForm = () => {
  emit("add-item", { id: nanoid(), event: form.value.text.trim() });
  console.log("已新增");
};
</script>

<template>
  <h2>正在編輯</h2>

  <form>
    <input type="text" v-model="form.text" />
    <button type="button" @click="editForm">送出</button>
  </form>
</template>
