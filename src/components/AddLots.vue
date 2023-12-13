<template>
    <div>
      <div class="d-flex align-items-center justify-content-center">
        <form id="id_form" class="col-md-6 shadow p-3 my-3" @submit.prevent="addTask">
          <label>Nouvelle tache:</label>
          <input v-model="newTask.lots" type="text" class="form-control">
          <button type="submit" class="btn btn-dark mt-3">Add</button>
        </form>
      </div>
    </div>
  </template>
  
  <script>
import axios from 'axios';

export default {
  data() {
    return {
      newTask: {
        lots: "",
      },
    };
  },
  methods: {
    async addTask() {
      try {
        // Effectuer la requête POST vers votre backend
        const response = await axios.post('http://localhost:3000/lots', this.newTask);

        // Logguer la réponse du serveur (peut être supprimé en production)
        console.log("Réponse du serveur:", response.data);

        // Réinitialiser la valeur de newTask.lots après l'ajout
        this.newTask.lots = "";

        // Émettre un événement vers le composant parent pour indiquer que la liste des tâches doit être mise à jour
        this.$emit('update-tasks');

      } catch (error) {
        console.error("Erreur lors de l'ajout de la tâche:", error);
      }
    },
  },
};
</script>
