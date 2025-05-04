<script setup>
import { ref, computed } from "vue";

// 組件
import Query from "../src/components/Query.vue";
import Add from "../src/components/Add.vue";
import Edit from "../src/components/Edit.vue";
import Delete from "../src/components/Delete.vue";

const currentView = ref("add");

const componentsMap = {
  query: Query,
  add: Add,
  edit: Edit,
  delete: Delete,
};

const todoList = ref([]); // 代辦事項儲存

// const editItem = ref([]);
const editItem = ref(null);

const currentComponent = computed(() => componentsMap[currentView.value]);

const updateView = (view) => {
  currentView.value = view;
};

const addList = (data) => {
  console.log(typeof data);
  todoList.value.push(data);
  console.log(todoList.value);
};

const checkDeleteItem = ref([]); // 記錄選中的項目

const checkItem = () => {
  console.log("5555", checkDeleteItem.value);
};

const deleteItem = () => {
  todoList.value = todoList.value.filter(
    (item) => !checkDeleteItem.value.includes(item.id)
  ); // item.id===>被checkbox選中項目
  console.log();
};

const tagEdit = (item) => {
  console.log("我點到事件了", item);

  editItem.value = item;

  console.log("888", editItem.value);
  updateView("edit");
};
</script>

<template>
  <div class="todolist">
    <h2>todolist</h2>
    <ul class="menu">
      <li>
        <button @click="updateView('add')">新增</button>
      </li>
      <li>
        <button @click="updateView('edit')">修改</button>
      </li>
      <li>
        <button @click="deleteItem()">刪除</button>
      </li>
    </ul>
    <div>
      <component
        :is="currentComponent"
        @update-view="updateView"
        @add-item="addList"
        :tagEdit="editItem"
        :key="editItem?.id ?? 'default'" 
      />
    </div>

    <ul class="list">
      <li v-for="item in todoList" :key="item.id" @dblclick="tagEdit(item)">
        <input
          type="checkbox"
          :value="item.id"
          v-model="checkDeleteItem"
          @change="checkItem"
        />{{ item.event }}
      </li>
    </ul>
  </div>
</template>

<style lang="scss" scoped>
.menu {
  display: flex;
  list-style: none;
  gap: 15px;
}
.list {
  display: flex;
  flex-direction: column;
  gap: 10px;
  li {
    border: 3px solid red;
  }
}
</style>
