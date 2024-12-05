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

<template>
  <div>
    <h1 class="text-2xl font-bold text-center my-4">Menu Cantine</h1>

    <div class="bg-gray-100 p-4 rounded shadow-md mb-6">
      <h2 class="text-lg font-semibold mb-4">Ajouter un menu</h2>
      <input
        type="text"
        v-model="starter"
        placeholder="Entrée"
        class="border p-2 rounded w-full mb-2"
      />
      <input
        type="text"
        v-model="MainCourse"
        placeholder="Plat"
        class="border p-2 rounded w-full mb-2"
      />
      <input
        type="text"
        v-model="Dessert"
        placeholder="Dessert"
        class="border p-2 rounded w-full mb-2"
      />
      <input
        type="text"
        v-model="note"
        placeholder="Note"
        class="border p-2 rounded w-full mb-2"
      />
      <button @click="add">
        Ajouter
      </button>
    </div>

    <div class="bg-gray-100 p-4 rounded shadow-md">
      <h2 class="text-lg font-semibold mb-4">Liste des menus</h2>
      <!-- Carrousel -->
      <div class="overflow-x-auto flex gap-4">
        <div
          v-for="(item, index) in ListMenu"
          :key="index"
          class="min-w-[250px] bg-white rounded shadow-md p-4 flex-shrink-0"
        >
          <h3 class="font-bold text-xl mb-2">Menu {{ index + 1 }}</h3>
          <p><strong>Entrée :</strong> {{ item.starter }}</p>
          <p><strong>Plat :</strong> {{ item.MainCourse }}</p>
          <p><strong>Dessert :</strong> {{ item.Dessert }}</p>
          <p><strong>Note :</strong> {{ item.note }} / 5</p>
        </div>
      </div>
    </div>
  </div>
</template>


<style>
</style>
