<template>
    <div class="weekly-menu">
      
        <h2>Menus de la semaine</h2>
  
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
        <button @click="addDish">Ajouter le menu</button>
      </div>

      <div class="counter">
        <h4>Nombre total de plats ajoutés : {{ totalDishes }}</h4>
      </div>
  
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
        // Liste des plats de la semaine
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
      // Calculer le nombre total de plats ajoutés
      totalDishes() {
        return this.weeklyMenus.reduce((total, menu) => {
          return total + (menu.starter ? 1 : 0) + (menu.mainCourse ? 1 : 0) + (menu.dessert ? 1 : 0);
        }, 0);
      }
    },
    methods: {
      // Fonction pour ajouter un plat
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
          // Réinitialiser les champs du formulaire
          this.newDish.starter = "";
          this.newDish.mainCourse = "";
          this.newDish.dessert = "";
        } else {
          alert("Veuillez remplir tous les champs !");
        }
      },
        // Fonction pour voter pour un plat
      vote(type, day) {
        this.weeklyMenus[day].votes[type]++;
      }
    }
  };
  </script>
  
  <style scoped>
  .weekly-menu {
    padding: 20px;
    font-family: 'Arial', sans-serif;
    background-color: #f4f7fa;
    border-radius: 8px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  }
  
  h2 {
    font-size: 2rem;
    color: #333;
    text-align: center;
    margin-bottom: 30px;
  }
  
  .counter {
    background-color: #f3f4f6;
    padding: 10px;
    margin-bottom: 30px;
    font-size: 1.2rem;
    color: #555;
    border-radius: 8px;
    display: flex;
    justify-content: center;
    font-weight: bold;
  }
  
  div > h3 {
    font-size: 1.4rem;
    margin-bottom: 15px;
    color: #333;
    text-align: center;
  }
  
  input {
    width: 100%;
    padding: 10px;
    margin: 10px 0;
    border-radius: 8px;
    border: 1px solid #ddd;
    font-size: 1rem;
    box-sizing: border-box;
  }
  
  input:focus {
    border-color: #4caf50;
    outline: none;
  }
  
  button {
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
  
  button:hover {
    background-color: #45a049;
  }

  .daily-menu {
    margin-bottom: 30px;
  }
  
  .daily-menu h3 {
    font-size: 1.5rem;
    color: #2c3e50;
    margin-bottom: 20px;
    text-align: center;
  }

  .menu-dishes {
    display: flex;
    justify-content: space-around;
    gap: 10px;
    flex-wrap: wrap;
  }
  </style>
  