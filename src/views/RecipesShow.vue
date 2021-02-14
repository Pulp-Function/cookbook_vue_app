<template>
  <div>
    <h2>{{ recipe.title }}</h2>
    <h4>{{ recipe.chef }}</h4>
    <p>{{ recipe.ingredients }}</p>
    <p>{{ recipe.directions }}</p>
    <a v-bind:href="`/recipes/${recipe.id}/edit`">Edit this recipe</a>
    <br />
    <a href="/recipes">Back to all recipes</a>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      recipe: {},
    };
  },
  created: function() {
    this.showRecipe();
  },
  methods: {
    showRecipe: function() {
      axios.get("/api/recipes/" + this.$route.params.id).then(response => {
        console.log(response.data);
        this.recipe = response.data;
      });
    },
  },
};
</script>
