<template>
  <div class="dish-card">
    <h4>{{ title }}</h4>
    <template v-if="isEditing">
      <input
        v-model="editableDescription"
        type="text"
        class="edit-input"
      />
      <button @click="saveEdit">Enregistrer</button>
      <br>
      <button @click="cancelEdit">Annuler</button>
    </template>
    <template v-else>
      <p>{{ description }}</p>
      <p><strong>Votes :</strong> {{ votes }}</p>
      <button @click="onVote">Voter</button>
      <br>
      <span>
        <button @click="startEdit">Modifier</button>
        |
        <button @click="onRemove">Supprimer</button>
      </span>
    </template>
  </div>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
      required: true,
    },
    description: {
      type: String,
      default: "Pas de description disponible.",
    },
    votes: {
      type: Number,
      required: true,
      default: 0,
    },
  },
  data() {
    return {
      isEditing: false,
      editableDescription: this.description,
    };
  },
  methods: {
    // Méthodes pour les boutons
    onVote() {
      this.$emit("vote");
    },
    // Émet un événement pour supprimer le plat
    onRemove() {
      this.$emit("remove");
    },
    // Émet un événement pour modifier la description
    startEdit() {
      this.isEditing = true;
    },
    // Émet un événement pour sauvegarder la nouvelle description
    saveEdit() {
      this.isEditing = false;
      this.$emit("edit", this.editableDescription);
    },

    // Annule l'édition et réinitialise la description
    cancelEdit() {
      this.isEditing = false;
      this.editableDescription = this.description;
    },
  },
};
</script>

<style scoped>
.dish-card {
  border: 1px solid #ddd;
  background-color: #F0F2F6;
  border-radius: 15px;
  width: 200px;
  height: auto;
  text-align: center;
  padding: 15px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}
button {
  background-color: #59A5D8;
  color: white;
  padding: 5px 10px;
  border: none;
  border-radius: 3px;
  cursor: pointer;
  margin: 5px 0;
}
button:hover {
  background-color: #386FA4;
}
.edit-input {
  width: 100%;
  padding: 5px;
  border: 1px solid #ddd;
  border-radius: 3px;
  margin-bottom: 5px;
}
</style>
