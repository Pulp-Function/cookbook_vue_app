<template>
  <div class="home">
    <h1>All recipes</h1>

    Search by title:
    <input v-model="titleFilter" type="text" list="recipe-titles" />
    <datalist id="recipe-titles">
      <option v-for="recipe in recipes" v-bind:key="recipe.id">{{ recipe.title }}</option>
    </datalist>

    <div class="row">
      <div class="col-sm-3" v-for="recipe in filterBy(recipes, titleFilter, 'title')" v-bind:key="recipe.id">
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
  object-fit: cover;
  height: 250px;
}
</style>

<script>
import axios from "axios";
import Vue2Filters from "vue2-filters";

export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      recipes: [],
      titleFilter: "",
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
