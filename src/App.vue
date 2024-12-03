<template>
  <div>
    <h1>Menu Cantine</h1>
  </div>
  <div>
    <input type="text" v-model="starter" placeholder="Entrée" />
    <input type="text" v-model="MainCourse" placeholder="Plat" />
    <input type="text" v-model="Dessert" placeholder="Dessert" />
    <button @click="add">Ajouter</button>
  </div>
  <div>
    <h2>Liste des menus</h2>
    <ul>
      <li v-for="(item, index) in ListMenu" :key="index">
        - Entrée : {{ item.starter }} - Plat : {{ item.MainCourse }} - Dessert :  {{ item.Dessert }}
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from "vue";

const starter = ref("");
const MainCourse = ref("");
const Dessert = ref("");

const ListMenu = ref([
  {
    starter: "Salade",
    MainCourse: "Poulet",
    Dessert: "Tarte aux pommes",
  },
]);

const add = () => {
  if (starter.value || MainCourse.value || Dessert.value) {
    ListMenu.value.push({
      starter: starter.value,
      MainCourse: MainCourse.value,
      Dessert: Dessert.value,
    });

    starter.value = "";
    MainCourse.value = "";
    Dessert.value = "";
  } else {
    alert("Veuillez remplir les champs !");
  }
};
</script>

<style>
</style>
