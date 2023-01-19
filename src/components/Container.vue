<template>
  <el-main class="main">
    <div class="content">
      <el-form @submit.native.prevent="onSubmit" ref="todo" :model="todo" label-width="120px"
               class="main-form">
        <el-input placeholder="Введите название задачи..." type="text"
                  v-model="todo.title" required></el-input>
        <el-button class="submit-button" type="success" native-type="submit">
          Добавить
        </el-button>
      </el-form>
      <List
        v-if="todos.length > 0"
        :todos="todos"
        @remove-task="removeTask"
        @update-task="updateTodos"
      ></List>
      <p v-else>Нет добавленных задач</p>
    </div>
  </el-main>
</template>

<script>
import axios from 'axios';
import List from '@/components/List.vue';

export default {
  name: 'Container',
  data() {
    return {
      todos: [],
      todo: {
        id: Date.now(),
        title: '',
        completed: false,
      },
    };
  },
  components: {
    List,
  },
  methods: {
    onSubmit() {
      this.todo.id = Date.now();
      this.postTodos(this.todo);
      this.todo = {
        title: '',
      };
    },
    removeTask(id) {
      this.deleteTodos(id);
    },
    async getTodos() {
      try {
        const res = await axios.get(this.VUE_APP_DB_URL);
        this.todos = res.data;
      } catch (e) {
        console.log('Ошибка загрузки');
      }
    },
    async postTodos(todo) {
      try {
        const newTodo = await axios.post(this.VUE_APP_DB_URL, todo);
        this.todos.push(newTodo.data);
      } catch (e) {
        console.log('Ошибка отправки');
      }
    },
    async deleteTodos(id) {
      try {
        await axios.delete(`${this.VUE_APP_DB_URL}/${id}`);
        this.todos = this.todos.filter((i) => i.id !== id);
      } catch (e) {
        console.log('Ошибка удаления');
      }
    },
    async updateTodos(todo) {
      try {
        const updatedTodo = await axios.patch(`${this.VUE_APP_DB_URL}/${todo.id}`, todo);
        this.todo = updatedTodo;
      } catch (e) {
        console.log('Ошибка обновления');
      }
    },
  },
  mounted() {
    this.VUE_APP_DB_URL = process.env.VUE_APP_DB_URL;
    this.getTodos();
  },
};
</script>

<style scoped>
.main {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.content {
  width: 800px;
  min-height: 500px;
  box-sizing: border-box;

  display: flex;
  flex-direction: column;
  align-items: center;

  border: 2px solid gray;
  border-radius: 10px;

  padding: 10px;
}

.main-form {
  width: 700px;

  display: flex;
  flex-direction: row;
}

.submit-button {
  margin-left: 10px;
}
</style>
