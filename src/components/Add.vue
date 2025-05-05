<script setup>
import { nanoid } from "nanoid";
import { ref } from "vue";
import { z } from "zod";
const emit = defineEmits(["update-view", "add-item"]);

const form = ref({
  text: "",
});

const zodForm = ref({
  mark: "", // 單一欄位驗證
});

const addSchema = z.object({
  text: z
    .string()
    .nonempty({ message: "不可空白" })
    .length(5, { message: "必須是文字且5個字" }),
  mark: z
    .string()
    .nonempty({ message: "不可空白" })
    .length(2, { message: "必須是文字且2個字" }),
});

// 整體驗證
const checkForm = () => {
  const result = addSchema.safeParse(form.value);
  console.log(result);
};

// 單欄驗證
// ✅ 單一欄位驗證（例如：zodForm.mark）
// 👉 注意！這裡用中括號 [field] 是為了「動態指定欄位名稱」
//    這樣你就可以驗 mark / text / 其他欄位，不需要重複寫多個函式
const checkSingle = (field) => {
  // const resultTWO = addSchema.shape.mark.safeParse(form.value.mark);
  const resultTWO = addSchema.shape[field].safeParse(zodForm.value[field]);
  console.log("====", resultTWO);
};

const addItem = () => {
  emit("add-item", { id: nanoid(), event: form.value.text.trim() });
  console.log("已新增");
};
</script>

<template>
  <h2>正在新增</h2>

  <form>
    <input type="text" @blur="checkForm" v-model="form.text" />

    <!-- 用 @blur 搭配 checkSingle('mark') 進行欄位驗證 -->
    <input type="text" @blur="checkSingle('mark')" v-model="zodForm.mark" />
    <button type="button" @click="addItem">送出</button>
  </form>
</template>
