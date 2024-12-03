<template>
  <div>
    <h1>Menu Cantine</h1>
  </div>
  <div>
    <input type="text" v-model="starter" placeholder="Entrée" /><br />
    <input type="text" v-model="MainCourse" placeholder="Plat" /><br />
    <input type="text" v-model="Dessert" placeholder="Dessert" /><br />
    <input type="text" v-model="note" placeholder="Note" />
    <button @click="add">Ajouter</button>
  </div> 
  <div>
    <h2>Liste des menus</h2>
    <ul>
      <li v-for="(item, index) in ListMenu" :key="index">
        - Entrée : {{ item.starter }} - Plat : {{ item.MainCourse }} - Dessert :  {{ item.Dessert }} - Note : {{ item.note }} / 5
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from "vue";

const starter = ref("");
const MainCourse = ref("");
const Dessert = ref("");
const note = ref("");

const ListMenu = ref([
  {
    starter: "Salade",
    MainCourse: "Poulet",
    Dessert: "Tarte aux pommes",
    note: "5",
    
  },
]);

const add = () => {
  if (starter.value || MainCourse.value || Dessert.value || note.value) {
    ListMenu.value.push({
      starter: starter.value,
      MainCourse: MainCourse.value,
      Dessert: Dessert.value,
      note: note.value,
    });

    starter.value = "";
    MainCourse.value = "";
    Dessert.value = "";
    note.value = "";
  } else {
    alert("Veuillez remplir les champs !");
  }
};
</script>

<style>
</style>
