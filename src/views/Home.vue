<template>
  <div class="home">
    <div>
      <h1>New Recipe</h1>
      Title:
      <input type="text" v-model="newRecipeTitle" />
      Chef:
      <input type="text" v-model="newRecipeChef" />
      Prep time:
      <input type="text" v-model="newRecipePrepTime" />
      Ingredients:
      <input type="text" v-model="newRecipeIngredients" />
      Directions:
      <input type="text" v-model="newRecipeDirections" />
      Image url:
      <input type="text" v-model="newRecipeImageUrl" />
      <button v-on:click="createRecipe()">Create</button>
    </div>
    <h1>All recipes</h1>
    <div v-for="recipe in recipes" v-bind:key="recipe.id">
      <h2>Title: {{ recipe.title }}</h2>
      <p>Chef: {{ recipe.chef }}</p>
      <img v-bind:src="recipe.image_url" alt="" />
      <div>
        <button v-on:click="showRecipe(recipe)">More info</button>
      </div>
    </div>

    <dialog id="recipe-details">
      <form method="dialog">
        <h1>Recipe info</h1>
        <p>
          Title:
          <input type="text" v-model="currentRecipe.title" />
        </p>
        <p>
          chef:
          <input type="text" v-model="currentRecipe.chef" />
        </p>
        <p>
          ingredients:
          <input type="text" v-model="currentRecipe.ingredients" />
        </p>
        <p>
          directions:
          <input type="text" v-model="currentRecipe.directions" />
        </p>
        <p>
          prep_time:
          <input type="text" v-model="currentRecipe.prep_time" />
        </p>
        <button v-on:click="updateRecipe(currentRecipe)">Update</button>
        <button v-on:click="destroyRecipe(currentRecipe)">Destroy</button>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>

<style>
img {
  width: 250px;
}
</style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      recipes: [],
      newRecipeTitle: "",
      newRecipeChef: "",
      newRecipePrepTime: "",
      newRecipeIngredients: "",
      newRecipeDirections: "",
      newRecipeImageUrl: "",
      currentRecipe: {},
    };
  },
  created: function() {
    this.indexRecipes();
  },
  methods: {
    indexRecipes: function() {
      axios.get("/api/recipes").then(response => {
        this.recipes = response.data;
        console.log("All recipes:", this.recipes);
      });
    },
    createRecipe: function() {
      console.log("createRecipe");
      var params = {
        title: this.newRecipeTitle,
        chef: this.newRecipeChef,
        prep_time: this.newRecipePrepTime,
        ingredients: this.newRecipeIngredients,
        directions: this.newRecipeDirections,
        image_url: this.newRecipeImageUrl,
      };
      axios
        .post("/api/recipes", params)
        .then(response => {
          console.log("Success", response.data);
          this.recipes.push(response.data);
        })
        .catch(error => console.log(error.response));
    },
    showRecipe: function(recipe) {
      console.log("showRecipe", recipe.title, recipe);
      this.currentRecipe = recipe;
      document.querySelector("#recipe-details").showModal();
    },
    updateRecipe: function(recipe) {
      var params = {
        title: recipe.title,
        chef: recipe.chef,
        ingredients: recipe.ingredients,
        directions: recipe.directions,
        prep_time: recipe.prep_time,
        image_url: recipe.image_url,
      };
      // axios.patch("/api/recipes/" + recipe.id, params).then(response => {
      axios.patch(`/api/recipes/${recipe.id}`, params).then(response => {
        console.log("Updated successfully", response.data);
      });
    },
    destroyRecipe: function(recipe) {
      axios.delete("/api/recipes/" + recipe.id).then(response => {
        console.log("Destroyed successfully", response.data);
        var index = this.recipes.indexOf(recipe);
        this.recipes.splice(index, 1);
      });
    },
  },
};
</script>
