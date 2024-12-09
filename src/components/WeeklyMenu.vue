<template>
    <div class="weekly-menu">
      
        <h2>Menus de la semaine</h2>
  
      <!-- Formulaire d'ajout de plat -->
      <div>
        <h3>Ajouter un plat</h3>
        <input
          type="text"
          v-model="newDish.starter"
          placeholder="Entrée"
        />
        <input
          type="text"
          v-model="newDish.mainCourse"
          placeholder="Plat"
        />
        <input
          type="text"
          v-model="newDish.dessert"
          placeholder="Dessert"
        />
        <button @click="addDish">Ajouter le plat</button>
      </div>

      <!-- Compteur du nombre de plats ajoutés en haut -->
      <div class="counter">
        <h4>Nombre total de plats ajoutés : {{ totalDishes }}</h4>
      </div>
  
      <!-- Liste des plats de la semaine -->
      <div v-for="(menu, day) in weeklyMenus" :key="day" class="daily-menu">
        <h3>Jour {{ day + 1 }}</h3>
        <div class="menu-dishes">
          <DishCard
            title="Entrée"
            :description="menu.starter"
            :votes="menu.votes.starter"
            @vote="vote('starter', day)"
          />
          <DishCard
            title="Plat"
            :description="menu.mainCourse"
            :votes="menu.votes.mainCourse"
            @vote="vote('mainCourse', day)"
          />
          <DishCard
            title="Dessert"
            :description="menu.dessert"
            :votes="menu.votes.dessert"
            @vote="vote('dessert', day)"
          />
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
        // Liste des menus pour chaque jour de la semaine
        weeklyMenus: [
          {
            starter: "Salade César",
            mainCourse: "Poulet rôti",
            dessert: "Tarte au citron",
            votes: { starter: 0, mainCourse: 0, dessert: 0 }
          },
          {
            starter: "Soupe de légumes",
            mainCourse: "Lasagnes",
            dessert: "Glace vanille",
            votes: { starter: 0, mainCourse: 0, dessert: 0 }
          },
          {
            starter: "Bruschetta",
            mainCourse: "Spaghetti bolognese",
            dessert: "Tiramisu",
            votes: { starter: 0, mainCourse: 0, dessert: 0 }
          }
        ],
        newDish: {
          starter: "",
          mainCourse: "",
          dessert: ""
        }
      };
    },
    computed: {
      // Compteur dynamique pour le nombre total de plats ajoutés
      totalDishes() {
        return this.weeklyMenus.reduce((total, menu) => {
          return total + (menu.starter ? 1 : 0) + (menu.mainCourse ? 1 : 0) + (menu.dessert ? 1 : 0);
        }, 0);
      }
    },
    methods: {
      // Ajouter un nouveau plat à la liste des menus
      addDish() {
        if (
          this.newDish.starter &&
          this.newDish.mainCourse &&
          this.newDish.dessert
        ) {
          this.weeklyMenus.push({
            starter: this.newDish.starter,
            mainCourse: this.newDish.mainCourse,
            dessert: this.newDish.dessert,
            votes: { starter: 0, mainCourse: 0, dessert: 0 }
          });
          // Réinitialiser le formulaire après ajout
          this.newDish.starter = "";
          this.newDish.mainCourse = "";
          this.newDish.dessert = "";
        } else {
          alert("Veuillez remplir tous les champs !");
        }
      },
  
      // Méthode pour voter pour un plat
      vote(type, day) {
        this.weeklyMenus[day].votes[type]++;
      }
    }
  };
  </script>
  
  <style scoped>
  .weekly-menu {
    padding: 20px;
  }
  
  .counter {
    margin-bottom: 20px;
    font-size: 1.2rem;
    font-weight: bold;
  }
  
  .daily-menu {
    margin-bottom: 20px;
  }
  
  .menu-dishes {
    display: flex;
    justify-content: space-around;
  }
  </style>
  