<template>
  <section>
    <div class="container">
      <div class="task">
        <!-- title -->
        <div class="title">
          <h1>Todo-List</h1>
        </div>
        <!-- form -->
        <div class="form">
          <!-- Menggunakan v-model untuk mengikat input dengan data 'newTask' -->
          <input type="text" placeholder="New Task" v-model="newTask" />
          <!-- Menggunakan <select> untuk memilih kategori -->
          <select v-model="newCategory" class="category-select">
            <option value="Work">Work</option>
            <option value="Education">Education</option>
            <option value="Finance">Finance</option>
            <option value="Health and Fitness">Health and Fitness</option>
            <option value="Events and Holidays">Events and Holidays</option>
            <option value="Routine Tasks">Routine Tasks</option>
          </select>
          <!-- Menggunakan @click untuk menambahkan tugas saat tombol "Tambah" diklik -->
          <button @click="addTask"><i class="fas fa-plus"></i></button>
        </div>
        <!-- task lists -->
        <div class="taskItems">
          <ul>
            <!-- Loop melalui array tasks dan menampilkan setiap tugas -->
            <li v-for="task in tasks" :key="task.id">
              <button @click="toggleCompleted(task.id)">
                <!-- Menggunakan ikon cek atau centang jika tugas selesai -->
                <i
                  class="far"
                  :class="{
                    'fa-check-circle': task.completed,
                    'fa-circle': !task.completed,
                  }"
                ></i>
              </button>
              {{ task.title }} (Category: {{ task.category }})
              <!-- Menampilkan kategori -->
              <!-- Menggunakan @click untuk menghapus tugas saat tombol "Hapus" diklik -->
              <button @click="deleteTask(task.id)">
                <i class="far fa-trash-alt"></i>
              </button>
            </li>
          </ul>
        </div>
        <!-- buttons -->
        <div class="clearBtns">
          <!-- Menggunakan @click untuk menjalankan metode clearCompleted saat tombol "Clear Completed" diklik -->
          <button @click="clearCompleted">Clear completed</button>
          <!-- Menggunakan @click untuk menjalankan metode clearAll saat tombol "Clear All" diklik -->
          <button @click="clearAll">Clear all</button>
        </div>
        <!-- pending task -->
        <div class="pendingTasks">
          <span>Pending Tasks: {{ pendingTasks }}</span>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: "TestTask",
  data() {
    return {
      newTask: "",
      newCategory: "",
      tasks: [],
    };
  },
  created() {
    // Cek apakah ada data di localStorage
    const storedTasks = localStorage.getItem("tasks");

    if (storedTasks) {
      // Jika ada data di localStorage, gunakan data tersebut
      this.tasks = JSON.parse(storedTasks);
    } else {
      // Jika tidak ada data di localStorage, gunakan data default
      this.tasks = [
        {
          id: 1,
          title: "Mengerjakan Tugas Vue.js",
          completed: false,
          category: "Education",
        },
      ];
    }
  },
  computed: {
    pendingTasks() {
      return this.tasks.filter((task) => !task.completed).length;
    },
  },
  methods: {
    addTask() {
      if (this.newTask.trim() !== "") {
        const newTask = {
          id: this.tasks.length + 1,
          title: this.newTask,
          completed: false,
          category: this.newCategory,
        };
        this.tasks.push(newTask);
        this.newTask = "";
        this.newCategory = "";

        // Simpan data ke localStorage
        this.saveTasksToLocalStorage();
      }
    },
    toggleCompleted(taskId) {
      const task = this.tasks.find((task) => task.id === taskId);
      if (task) {
        task.completed = !task.completed;

        // Update data di localStorage setelah mengganti status tugas
        this.saveTasksToLocalStorage();
      }
    },
    deleteTask(taskId) {
      this.tasks = this.tasks.filter((task) => task.id !== taskId);

      // Update data di localStorage setelah menghapus tugas
      this.saveTasksToLocalStorage();
    },
    clearCompleted() {
      this.tasks = this.tasks.filter((task) => !task.completed);

      // Update data di localStorage setelah menghapus tugas yang sudah selesai
      this.saveTasksToLocalStorage();
    },
    clearAll() {
      this.tasks = [];

      // Hapus semua data di localStorage
      localStorage.removeItem("tasks");
    },
    saveTasksToLocalStorage() {
      localStorage.setItem("tasks", JSON.stringify(this.tasks));
    },
  },
};
</script>

<style scoped>
/* Gaya untuk elemen <select> */
.category-select {
  width: 100%;
  height: 50px;
  font: 15px/1.4 "Poppins", sans-serif;
  padding: 15px;
  background: #f3f3f388;
  color: #000000;
  border: 1px solid transparent;
  border-radius: 10px;
  transition: border 0.3s linear;
}

.category-select:focus {
  outline: none;
  border: 1px solid #904ec5;
}

.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh; /* Sesuaikan tinggi sesuai kebutuhan */
}
</style>
