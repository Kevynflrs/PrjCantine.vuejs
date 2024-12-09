<template>
  <div class="weekly-menu">
    <h2>Menus de la semaine</h2>

    <div class="counter">
      <h4>Nombre total de plats ajoutés : {{ totalDishes }}</h4>
    </div>

    <div class="day-selector">
      <h4>Choisir le jour</h4>
      <select v-model="selectedDay">
        <option :value="null" disabled>Sélectionnez un jour</option>
        <option v-for="(menu, day) in weeklyMenus" :key="day" :value="day">
          {{ daysOfWeek[day] }}
        </option>
      </select>
    </div>

    <div v-if="selectedDay !== null" :key="selectedDay" class="daily-menu">
      <h3>{{ daysOfWeek[selectedDay] }}</h3>

      <div v-for="(starter, index) in weeklyMenus[selectedDay].starters" :key="'starter-' + index">
        <DishCard
          title="Entrée"
          :description="starter"
          :votes="weeklyMenus[selectedDay].votes.starter"
          @vote="vote('starter', selectedDay)"
        />
      </div>

      <div v-for="(main, index) in weeklyMenus[selectedDay].mainCourses" :key="'mainCourse-' + index">
        <DishCard
          title="Plat"
          :description="main"
          :votes="weeklyMenus[selectedDay].votes.mainCourse"
          @vote="vote('mainCourse', selectedDay)"
        />
      </div>

      <div v-for="(dessert, index) in weeklyMenus[selectedDay].desserts" :key="'dessert-' + index">
        <DishCard
          title="Dessert"
          :description="dessert"
          :votes="weeklyMenus[selectedDay].votes.dessert"
          @vote="vote('dessert', selectedDay)"
        />
      </div>
    </div>

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
    </div>
  </div>
</template>

<script>
// Importer le composant DishCard
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
      // Tableau des jours de la semaine
      daysOfWeek: ["Lundi", "Mardi", "Mercredi", "Jeudi", "Vendredi", "Samedi", "Dimanche"],
    };
  },
  computed: {
    totalDishes() {
      return this.weeklyMenus.reduce((total, menu) => {
        return total +
          (menu.starters.length) +
          (menu.mainCourses.length) +
          (menu.desserts.length);
      }, 0);
    }
  },
  methods: {
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

        // On map les plats récupérés de l'API pour les organiser par type de plat
        this.weeklyMenus = this.daysOfWeek.map(() => {
          return {
            starters: this.getRandomDishes(dishes, 2),      // 2 entrées par jour
            mainCourses: this.getRandomDishes(dishes, 3),   // 3 plats par jour
            desserts: this.getRandomDishes(dishes, 2),      // 2 desserts par jour
            votes: { starter: 0, mainCourse: 0, dessert: 0 }
          };
        });
      } catch (error) {
        console.error("Erreur lors de la récupération des plats de l'API", error);
      }
    },

    // Fonction pour obtenir un nombre donné de plats aléatoires
    getRandomDishes(dishes, count) {
      const shuffled = dishes.sort(() => 0.5 - Math.random());
      return shuffled.slice(0, count).map(dish => dish.name);
    },

    addDish() {
      if (
        this.newDish.starter &&
        this.newDish.mainCourse &&
        this.newDish.dessert &&
        this.dishDay !== null
      ) {
        this.weeklyMenus[this.dishDay].starters.push(this.newDish.starter);
        this.weeklyMenus[this.dishDay].mainCourses.push(this.newDish.mainCourse);
        this.weeklyMenus[this.dishDay].desserts.push(this.newDish.dessert);

        this.newDish.starter = "";
        this.newDish.mainCourse = "";
        this.newDish.dessert = "";
        this.dishDay = null;
      } else {
        alert("Veuillez remplir tous les champs et choisir un jour !");
      }
    },

    vote(type, day) {
      this.weeklyMenus[day].votes[type]++;
    }
  },

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
  border-color: #4caf50;
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
  border-color: #4caf50;
  outline: none;
}

.add-button-container {
  margin-top: 10px;
}

.add-button {
  width: 100%;
  padding: 12px;
  background-color: #4caf50;
  color: white;
  font-size: 1.2rem;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.add-button:hover {
  background-color: #45a049;
}
</style>
