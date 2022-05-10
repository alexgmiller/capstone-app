<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "Welcome to GameNet!",
      games: [],
    };
  },
  created: function () {
    this.indexGames();
  },
  methods: {
    indexRecipes: function () {
      axios.get("http://localhost:3000/recipes.json").then((response) => {
        this.recipes = response.data;
        console.log("All recipes", this.recipes);
      });
    },
    createRecipe: function () {
      console.log("Making a new recipe!");

      axios
        .post("http://localhost:3000/recipes.json", this.newRecipeParams)
        .then((response) => {
          console.log("Success", response.data);
          this.recipes.push(response.data);
        })
        .catch((error) => console.log(error.response)); //error handling -> needs happy/sad paths from rails!!!
    },
    showRecipe: function (recipe) {
      console.log(recipe);
      this.currentRecipe = recipe;
      document.querySelector("#recipe-details").showModal();
    },
    updateRecipe: function (recipe) {
      var editRecipeParams = recipe;
      axios.patch("http://localhost:3000/recipes/" + recipe.id + ".json", editRecipeParams).then((response) => {
        console.log("Success!", response.data);
      });
    },
    destroyRecipe: function (recipe) {
      axios.delete("http://localhost:3000/recipes/" + recipe.id).then((response) => {
        console.log("Good job!", response.data);
        var index = this.recipes.indexOf(recipe);
        this.recipes.splice(index, 1);
      });
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
  </div>
</template>

<style></style>
