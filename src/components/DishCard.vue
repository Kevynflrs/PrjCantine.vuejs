<template>
    <div class="dish-card">
      <h4>{{ title }}</h4>
      <p>{{ description }}</p>
      <p><strong>Votes :</strong> {{ votes }}</p>
      <button @click="onVote">Voter</button>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      title: {
        type: String,
        required: true
      },
      description: {
        type: String,
        default: "Pas de description disponible."
      },
      votes: {
        type: Number,
        required: true,
        default: 0
      }
    },
    methods: {
      onVote() {
        this.$emit("vote");
      }
    }
  };
  </script>
  
  <style scoped>
  .dish-card {
    border: 1px solid #ddd;
    background-color: #F0F2F6;
    border-radius: 15px;
    width: 200px;
    height: 200px;
    text-align: center;
  }
  button {
    background-color: #59A5D8;
    color: white;
    padding: 5px 10px;
    border: none;
    border-radius: 3px;
    cursor: pointer;
  }
  button:hover {
    background-color: #386FA4;
  }
  </style>
  