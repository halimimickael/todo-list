<template>
  <div>
    <h1 class="display-1 text-center">Todo List</h1>

    <div class="container mt-5">
      <div class="row d-flex justify-content-center">
        <div class="col-md-6">
          <div class="card shadow">
            <div class="card-body">
              <ul class="list-group list-group-flush">
                <li
                  v-for="task in tasks"
                  :key="task._id"
                  class="list-group-item d-flex justify-content-between align-items-center"
                >
                  {{ task.lots }}
                  <button class="btn btn-close" @click="deleteLot(task._id)"></button>
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="d-flex align-items-center justify-content-center">
      <button class="btn btn-dark mt-3" @click="toggleAddLot">+</button>
    </div>
    <!-- Conditionally render AddTaches component based on showAddLot value -->
    <AddLots v-if="showAddLot" @updateTasks="fetchTasks" />
  </div>
</template>

<script>
import AddLots from "./AddLots.vue";

export default {
  data() {
    return {
      showAddLot: false,
      tasks: [],
    };
  },
  methods: {
    toggleAddLot() {
      this.showAddLot = !this.showAddLot;
    },
    async fetchTasks() {
      try {
        const response = await fetch("http://localhost:3000/lots");
        const data = await response.json();
        this.tasks = data;
      } catch (error) {
        console.error("Error fetching tasks:", error);
      }
    },
    async deleteLot(id) {
      try {
        await fetch(`http://localhost:3000/lots/${id}`, {
          method: "DELETE",
        });
        // Réactualiser la liste des tâches après la suppression
        this.fetchTasks();
      } catch (error) {
        console.error("Error deleting lot:", error);
      }
    },
  },

  mounted() {
    // Fetch tasks when the component is mounted
    this.fetchTasks();
  },
  components: {
    AddLots,
  },
};
</script>

<style>
/* h1{
  text-align: center;
} */
</style>
