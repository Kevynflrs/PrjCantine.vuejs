<template>
  <div class="weekly-menu">
    <div class="add-dish-form">
      <h3>Ajouter un plat</h3>
      <select v-model="dishDay" class="dish-select">
        <option value="" disabled>Sélectionner un jour</option>
        <option v-for="(menu, day) in weeklyMenus" :key="day" :value="day">
          {{ daysOfWeek[day] }}
        </option>
      </select>
      <input
        type="text"
        v-model="newDish.starter"
        placeholder="Entrée"
        class="dish-input"
      />
      <input
        type="text"
        v-model="newDish.mainCourse"
        placeholder="Plat"
        class="dish-input"
      />
      <input
        type="text"
        v-model="newDish.dessert"
        placeholder="Dessert"
        class="dish-input"
      />
      <div class="add-button-container">
        <button @click="addDish" class="add-button">Ajouter le menu</button>
      </div>

      <div class="counter">
        <h4>Nombre total de plats ajoutés : {{ totalDishes() }}</h4>
      </div>
    </div>

    <br>

    <div class="day-selector">
      <div class="day-buttons">
        <button
          v-for="(day, index) in daysOfWeek"
          :key="index"
          :class="['day-button', { active: selectedDay === index }]"
          @click="selectedDay = index"
        >
        {{ day }}
        </button>
      </div>
    </div>

    <div v-if="selectedDay !== null" :key="selectedDay" class="daily-menu">
      <div class="row row-starters">
        <div
          v-for="(starter, index) in weeklyMenus[selectedDay].starters"
          :key="'starter-' + index"
          class="dish-card-container"
        >
          <DishCard
            title="Entrée"
            :description="starter.name"
            :votes="starter.votes"
            @vote="vote(starter)"
            @edit="updateDishDescription('starter', index, $event)"
            @remove="removeDish('starter', index)"
          />
        </div>
      </div>
      <div class="row row-main-courses">
        <div
          v-for="(main, index) in weeklyMenus[selectedDay].mainCourses"
          :key="'mainCourse-' + index"
          class="dish-card-container"
        >
          <DishCard
            title="Plat"
            :description="main.name"
            :votes="main.votes"
            @vote="vote(main)"
            @edit="updateDishDescription('mainCourse', index, $event)"
            @remove="removeDish('mainCourse', index)"
          />
        </div>
      </div>
      <div class="row row-desserts">
        <div
          v-for="(dessert, index) in weeklyMenus[selectedDay].desserts"
          :key="'dessert-' + index"
          class="dish-card-container"
        >
          <DishCard
            title="Dessert"
            :description="dessert.name"
            :votes="dessert.votes"
            @vote="vote(dessert)"
            @edit="updateDishDescription('dessert', index, $event)"
            @remove="removeDish('dessert', index)"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import DishCard from "./DishCard.vue";

export default {
  components: {
    DishCard
  },
  data() {
  return {
    weeklyMenus: [],
    newDish: {
      starter: "",
      mainCourse: "",
      dessert: ""
    },
    selectedDay: null,
    dishDay: null,
    daysOfWeek: ["Lundi", "Mardi", "Mercredi", "Jeudi", "Vendredi", "Samedi", "Dimanche"]
  };
},
methods: {

  // Méthode pour récupérer les plats de l'API
  async fetchPlats() {
    try {
      const response = await fetch('https://tasty.p.rapidapi.com/recipes/list?from=0&size=20&tags=under_30_minutes', {
        method: 'GET',
        headers: {
          'x-rapidapi-key': 'd6aac1fc7bmshadab676da5d5366p1f0beajsn6cb4f1dd1974',
          'x-rapidapi-host': 'tasty.p.rapidapi.com',
        }
      });
      const data = await response.json();
      const dishes = data.results;

      this.weeklyMenus = this.daysOfWeek.map(() => ({
        starters: this.getRandomDishes(dishes, 2),
        mainCourses: this.getRandomDishes(dishes, 3),
        desserts: this.getRandomDishes(dishes, 2)
      }));
    } catch (error) {
      console.error("Erreur lors de la récupération des plats de l'API", error);
    }
  },

  // méthode pour obtenir un nombre aléatoire de plats
  getRandomDishes(dishes, count) {
    const shuffled = dishes.sort(() => 0.5 - Math.random());
    return shuffled.slice(0, count).map(dish => ({ name: dish.name, votes: 0 }));
  },

  // Méthode pour éditer un plat
  editDish(dish, type, index) {
    const newName = prompt(`Modifier le nom du plat (${dish.name}):`, dish.name);
    if (newName !== null && newName.trim() !== "") {
      this.weeklyMenus[this.selectedDay][type + 's'][index].name = newName.trim();
    }
  },

  // Méthode pour mettre à jour la description d'un plat
  updateDishDescription(type, index, newDescription) {
    this.weeklyMenus[this.selectedDay][type + 's'][index].name = newDescription;
  },

  // Méthode pour supprimer un plat
  removeDish(type, index) {
    if (this.selectedDay !== null) {
      this.weeklyMenus[this.selectedDay][type + 's'].splice(index, 1);
    }
  },

  // Méthode pour calculer le nombre total de plats
  totalDishes() {
    let count = 0;
    this.weeklyMenus.forEach(menu => {
      count += menu.starters.length + menu.mainCourses.length + menu.desserts.length;
    });
    return count;
  },

  // Méthode pour ajouter un plat 
  addDish() {
    if (
      (this.newDish.starter || this.newDish.mainCourse || this.newDish.dessert) &&
      this.dishDay !== null
    ) {
      if (this.newDish.starter) {
        this.weeklyMenus[this.dishDay].starters.push({ name: this.newDish.starter, votes: 0 });
      }
      if (this.newDish.mainCourse) {
        this.weeklyMenus[this.dishDay].mainCourses.push({ name: this.newDish.mainCourse, votes: 0 });
      }
      if (this.newDish.dessert) {
        this.weeklyMenus[this.dishDay].desserts.push({ name: this.newDish.dessert, votes: 0 });
      }
      this.newDish.starter = "";
      this.newDish.mainCourse = "";
      this.newDish.dessert = "";
      this.dishDay = null;
    } else {
      alert("Veuillez remplir au moins un champ (Entrée, Plat ou Dessert) et choisir un jour !");
    }
  },

  // Méthode pour voter pour un plat
  vote(dish) {
    dish.votes++;
  }
},

  // Appel de la méthode fetchPlats lors de la création du composant
  mounted() {
    this.fetchPlats();
  }
};
</script>

<style scoped>
.add-dish-form {
  background-color: #f3f4f6;
  padding: 15px;
  margin-top: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.add-dish-form h3 {
  font-size: 1.4rem;
  color: #333;
  margin-bottom: 15px;
  text-align: center;
}

.dish-input {
  width: 100%;
  padding: 10px;
  margin: 10px 0;
  border-radius: 8px;
  border: 1px solid #ddd;
  font-size: 1rem;
  box-sizing: border-box;
}

.dish-input:focus {
  border-color: #59A5D8;
  outline: none;
}

.dish-select {
  width: 100%;
  padding: 10px;
  margin: 10px 0;
  border-radius: 8px;
  border: 1px solid #ddd;
  font-size: 1rem;
  box-sizing: border-box;
}

.dish-select:focus {
  border-color: #59A5D8;
  outline: none;
}

.add-button-container {
  margin-top: 10px;
}

.add-button {
  width: 100%;
  padding: 12px;
  background-color: #59A5D8;
  color: white;
  font-size: 1.2rem;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.add-button:hover {
  background-color: #386FA4;
}

.row {
  margin-bottom: 20px;
  padding: 10px;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
}

.row h4 {
  text-align: left;
  font-size: 1.2rem;
  color: #444;
  margin-bottom: 10px;
  width: 100%;
}

.dish-card-container {
  display: inline-block;
  margin: 10px;
}

.dish-card {
  width: 180px;
  border: 1px solid #ddd;
  background-color: #f0f2f6;
  border-radius: 8px;
  padding: 15px;
  text-align: center;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.dish-card h4 {
  font-size: 1rem;
  margin-bottom: 10px;
}

.dish-card p {
  font-size: 0.9rem;
  margin: 5px 0;
}

button {
  background-color: #59a5d8;
  color: white;
  padding: 5px 10px;
  border: none;
  border-radius: 3px;
  cursor: pointer;
}

button:hover {
  background-color: #386fa4;
}

.day-buttons {
  display: flex;
  gap: 10px;
  justify-content: center;
  margin-bottom: 20px;
}

.day-button {
  padding: 10px 15px;
  background-color: #f0f2f6;
  color: #444;
  border: 1px solid #ddd;
  border-radius: 5px;
  font-size: 1rem;
  cursor: pointer;
  transition: background-color 0.3s ease, color 0.3s ease;
}

.day-button.active {
  background-color: #59a5d8;
  color: white;
  font-weight: bold;
}

.day-button:hover {
  background-color: #386fa4;
  color: white;
}
</style>
