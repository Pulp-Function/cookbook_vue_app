<template>
  <div class="home">
    <h1>All recipes</h1>

    <div class="row">
      <div class="col-sm-3" v-for="recipe in recipes" v-bind:key="recipe.id">
        <div class="card">
          <img v-bind:src="recipe.image_url" class="card-img-top" alt="..." />
          <div class="card-body">
            <h5 class="card-title">{{ recipe.title }}</h5>
            <p class="card-text">Chef: {{ recipe.chef }}</p>
            <a class="btn btn-primary" v-bind:href="`/recipes/${recipe.id}`">More info</a>
          </div>
        </div>
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
