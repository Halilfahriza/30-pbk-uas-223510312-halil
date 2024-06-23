<template>
  <div>
    <!-- Header dengan Navigation -->
    <header class="header">
      <nav class="menu">
        <ul>
          <li><router-link to="/todos">Todos</router-link></li>
          <li><router-link to="/posts">Posts</router-link></li>
          <li><router-link to="/albums">Albums</router-link></li>
        </ul>
      </nav>
    </header>
    
    <!-- Router View untuk Komponen Lain -->
    <router-view></router-view>

    <!-- Kontainer Todo List -->
    <div class="container py-5">
      <div class="row d-flex justify-content-center align-items-center">
        <div class="col-10">
          <div class="card rounded shadow-sm">
            <div class="card-body p-5">
              <!-- Judul dan Total Tugas -->
              <h3 class="mb-3 fw-bold">
                To-Do List
                <span class="badge bg-primary rounded-pill ms-3 fs-6 fw-normal">{{ totalTodo }} tasks</span>
              </h3>

              <!-- Form Todo -->
              <form @submit.prevent="addTodo" class="row align-items-center mb-3 gy-2">
                <div class="col-sm-12 col-md-10">
                  <div class="form-floating">
                    <input type="text" class="form-control fs-4" id="floatingInput" placeholder="Todos" autocomplete="off" v-model="todo" />
                    <label for="floatingInput">Apa rencana kegiatanmu hari ini?</label>
                  </div>
                </div>
                <div class="col-sm-12 col-md-2">
                  <div class="d-grid">
                    <button type="submit" class="btn btn-primary btn-lg" :disabled="!todo.trim()">
                      <font-awesome-icon icon="circle-plus" />
                      Tambah
                    </button>
                  </div>
                </div>
              </form>

              <!-- Daftar Todo -->
              <ul class="list-group list-group-flush">
                <li v-for="(todo, index) in todos" :key="index" class="list-group-item">
                  <div class="row gy-2 align-items-center">
                    <div class="col-sm-12 col-md-auto me-auto fs-5" :class="{ 'text-decoration-line-through text-muted': todo.isDone }" v-if="editedTodoIndex !== index">
                      {{ todo.activity }}
                    </div>
                    <div class="col-sm-12 col-md-auto" v-else>
                      <input v-model="editedTodoText" class="form-control" @keyup.enter="saveEdit(index)" @keyup.escape="cancelEdit">
                    </div>
                    <div class="col-sm-12 col-md-auto">
                      <div class="row gx-2">
                        <div class="col-auto">
                          <button class="btn" :class="[todo.isDone ? 'btn-outline-secondary' : 'btn-outline-success']" @click="doneTodo(index)">
                            <font-awesome-icon icon="circle-check" />
                          </button>
                        </div>
                        <div class="col-auto">
                          <button class="btn btn-outline-danger" @click="confirmDelete(index)">
                            <font-awesome-icon icon="eraser" />
                          </button>
                        </div>
                        <div class="col-auto" v-if="editedTodoIndex !== index">
                          <button class="btn btn-outline-primary" @click="editTodo(index, todo.activity)">
                            <font-awesome-icon icon="edit" />
                          </button>
                        </div>
                        <div class="col-auto" v-else>
                          <button class="btn btn-outline-secondary" @click="cancelEdit">
                            <font-awesome-icon icon="times" />
                          </button>
                          <button class="btn btn-outline-success" @click="saveEdit(index)">
                            <font-awesome-icon icon="check" />
                          </button>
                        </div>
                      </div>
                    </div>
                  </div>
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      todo: '',
      todos: [],
      editedTodoIndex: null,
      editedTodoText: '',
    };
  },
  computed: {
    totalTodo() {
      return this.todos.length;
    },
  },
  methods: {
    addTodo() {
      if (this.todo.trim()) {
        this.todos.push({ activity: this.todo.trim(), isDone: false });
        this.todo = '';
      } else {
        alert('Todo tidak boleh kosong');
      }
    },
    doneTodo(index) {
      this.todos[index].isDone = !this.todos[index].isDone;
    },
    confirmDelete(index) {
      if (confirm('Apakah Anda yakin ingin menghapus todo ini?')) {
        this.deleteTodo(index);
      }
    },
    deleteTodo(index) {
      this.todos.splice(index, 1);
    },
    editTodo(index, text) {
      this.editedTodoIndex = index;
      this.editedTodoText = text;
    },
    saveEdit(index) {
      if (this.editedTodoText.trim()) {
        this.todos[index].activity = this.editedTodoText.trim();
        this.cancelEdit();
      } else {
        alert('Todo tidak boleh kosong');
      }
    },
    cancelEdit() {
      this.editedTodoIndex = null;
      this.editedTodoText = '';
    },
  },
};
</script>

<style scoped>
/* General Styles */
body {
  font-family: 'Arial', sans-serif;
  background-color: #f4f4f4;
  color: #333;
}

.header {
  background-color: #3b5998;
  padding: 1rem;
}

.menu ul {
  display: flex;
  list-style: none;
  justify-content: center;
  font-family: 'Times New Roman', serif;
}

.menu li {
  margin: 0 1rem;
}

.menu a {
  color: #fff;
  text-decoration: none;
  font-weight: bold;
}

.menu a:hover {
  color: #f4f4f4;
}

/* Todo List Styles */
.card {
  background-color: #fff;
  border: none;
  border-radius: 10px;
}

.card-body {
  padding: 2rem;
}

h3 {
  font-family: 'Georgia', serif;
  color: #333;
}

.btn-primary {
  background-color: #3b5998;
  border: none;
}

.btn-primary:hover {
  background-color: #2d4373;
}

.list-group-item {
  background-color: #fff;
  border: none;
  border-bottom: 1px solid #ddd;
}

.list-group-item:last-child {
  border-bottom: none;
}

.btn {
  transition: all 0.3s ease;
}

.btn-outline-primary:hover,
.btn-outline-success:hover,
.btn-outline-danger:hover {
  color: #fff;
}

.btn-outline-primary:hover {
  background-color: #3b5998;
}

.btn-outline-success:hover {
  background-color: #28a745;
}

.btn-outline-danger:hover {
  background-color: #dc3545;
}

input.form-control {
  border: 1px solid #ccc;
  transition: border-color 0.3s ease;
}

input.form-control:focus {
  border-color: #3b5998;
  box-shadow: none;
}

.card:hover {
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
}

.card-body {
  transition: transform 0.3s ease;
}

.card:hover .card-body {
  transform: scale(1.02);
}

.card-header {
  background-color: #3b5998;
  color: #fff;
  border-radius: 10px 10px 0 0;
}
</style>
