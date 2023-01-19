<template>
  <div>
    <ul class="tasks-list">
      <li
        v-for="todo of todos"
        :key="todo.id"
        class="task"
      >
        <div>
          <el-form @submit.native.prevent="updateTask(todo)" ref="todo" :model="todo"
                   class="task-container">
            <el-checkbox name="type"
                         v-model="todo.completed"
                         @change="updateTask(todo)"
            ></el-checkbox>
            <el-input
              v-model="todo.title"
              type="text"
              clearable
            ></el-input>
          </el-form>
        </div>
        <el-button size="mini" type="primary" icon="el-icon-delete"
                   @click="removeTask(todo.id)"></el-button>
      </li>
    </ul>
  </div>
</template>

<script>

export default {
  name: 'List',
  props: {
    todos: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {};
  },
  methods: {
    removeTask(id) {
      this.$emit('remove-task', id);
    },
    updateTask(todo) {
      this.$emit('update-task', todo);
    },
  },
};
</script>

<style scoped>
.task-container {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;

  gap: 10px;
}

.tasks-list {
  list-style: none;

  width: 680px;
  box-sizing: border-box;

  margin: 0;
  padding: 10px;
}

.task {
  width: 100%;
  margin: 15px 5px 15px 5px;
  padding-bottom: 5px;

  display: flex;
  align-items: flex-start;
  justify-content: space-between;

  border-bottom: 1px solid lightblue;
}
</style>
