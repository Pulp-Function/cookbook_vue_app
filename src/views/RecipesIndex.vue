<template>
  <div class="home">
    <h1>All recipes</h1>
    <div v-for="recipe in recipes" v-bind:key="recipe.id">
      <h2>Title: {{ recipe.title }}</h2>
      <p>Chef: {{ recipe.chef }}</p>
      <img v-bind:src="recipe.image_url" alt="" />
      <div>
        <a v-bind:href="`/recipes/${recipe.id}`">More info</a>
      </div>
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
  },
};
</script>
