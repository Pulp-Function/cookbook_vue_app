<template>
  <div class="home">
    <div>
      <h1>New Recipe</h1>
      <button v-on:click="createRecipe()">Create</button>
    </div>
    <h1>All recipes</h1>
    <div v-for="recipe in recipes" v-bind:key="recipe.id">
      <h2>Title: {{ recipe.title }}</h2>
      <p>Chef: {{ recipe.chef }}</p>
      <img v-bind:src="recipe.image_url" alt="" />
    </div>
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
        title: "example title",
        chef: "example chef",
        prep_time: 32,
        ingredients: "one, two, three",
        directions: "cook, serve",
        image_url: "example image",
      };
      axios
        .post("/api/recipes", params)
        .then(response => {
          console.log("Success", response.data);
          this.recipes.push(response.data);
        })
        .catch(error => console.log(error.response));
    },
  },
};
</script>
