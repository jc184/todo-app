<template>
  <div class="mb-4">
    <div class="mb-4">
      <h1>{{ about.title }}</h1>
      <h2>{{ about.subTitle }} {{ version }}</h2>
    </div>
  </div>
  <form @submit.prevent="addNewTodo">
    <div class="mb-5">
      <label for="newTodo" class="form-label">New Todo</label>
      <input
        class="form-control"
        id="newTodo"
        placeholder="what's on your mind?"
        v-model="newTodo"
        name="newTodo"
      />
      <div class="m-2">
        <button class="btn btn-primary" type="submit">Add New Todo</button>
      </div>
      <div class="m-2">
        <button type="button" class="btn btn-danger" @click="removeAllTodos">
          Remove All
        </button>
      </div>
      <div class="m-2">
        <button type="button" class="btn btn-success" @click="markAllDone">
          Mark All Done
        </button>
      </div>
    </div>
  </form>
  <div v-if="todos.length === 0">
    <h3>Empty list 🥺</h3>
  </div>
  <div v-else>
    <ul class="list-group">
      <li
        class="list-group-item d-flex flex-row justify-content-between align-items-center"
        v-for="(todo, index) in todos"
        :key="todo.id"
      >
        <h3
          :class="{ mark: todo.done }"
          style="cursor: pointer"
          @click="toggleDone(todo)"
        >
          {{ todo.content }}
        </h3>
        <button
          type="button"
          class="btn btn-warning"
          @click="removeTodo(index)"
        >
          ✔️ Done & Remove
        </button>
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, PropType, onMounted } from "vue";
import { TodoType } from "@/models/todoModel";
import { v4 as uuidv4 } from "uuid";
type Props = {
  title: string;
  subTitle: string;
};

export default defineComponent({
  name: "TodoForm",

  props: {
    about: {
      type: Object as PropType<Props>,
      required: true,
    },
  },
  setup(props) {
    // local state
    const version = ref("v1");
    const newTodo = ref("");

    const todos = ref<TodoType[]>([]);
    function addNewTodo(): void {
      if (!newTodo.value) return;
      todos.value.push({
        id: uuidv4(),
        done: false,
        content: newTodo.value,
      });
      console.log("newTodo:", newTodo.value);
      newTodo.value = "";
    }
    function toggleDone(todo: TodoType): void {
      todo.done = !todo.done;
    }
    function removeTodo(index: number): void {
      todos.value.splice(index, 1);
    }
    function markAllDone(): void {
      todos.value.forEach((todo) => (todo.done = true));
    }
    function removeAllTodos(): void {
      todos.value = [];
    }
    onMounted(() => console.log(props.about.title));
    return {
      version,
      todos,
      newTodo,
      addNewTodo,
      toggleDone,
      removeTodo,
      markAllDone,
      removeAllTodos,
    };
  },
});
</script>
<style>
.mark {
  text-decoration: line-through;
}
</style>
