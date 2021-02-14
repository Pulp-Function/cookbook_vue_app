<template>
  <div class="recipes-edit">
    <form v-on:submit.prevent="updateRecipe()">
      <h1>Edit Recipe</h1>
      <ul>
        <li class="text-danger" v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>
      <div class="form-group">
        <label>Title:</label>
        <input type="text" class="form-control" v-model="title" />
      </div>
      <div class="form-group">
        <label>Chef:</label>
        <input type="text" class="form-control" v-model="chef" />
      </div>
      <div class="form-group">
        <label>Prep time:</label>
        <input type="text" class="form-control" v-model="prepTime" />
      </div>
      <div class="form-group">
        <label>Ingredients:</label>
        <input type="text" class="form-control" v-model="ingredients" />
      </div>
      <div class="form-group">
        <label>Directions:</label>
        <input type="text" class="form-control" v-model="directions" />
      </div>
      <div class="form-group">
        <label>Image url:</label>
        <input type="text" class="form-control" v-model="imageUrl" />
      </div>
      <input type="submit" class="btn btn-primary" value="Submit" />
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      recipe: {},
      title: "Corn on the cob",
      chef: "",
      prepTime: "",
      ingredients: "",
      directions: "",
      imageUrl: "",
      errors: [],
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
        this.title = this.recipe.title;
        this.chef = this.recipe.chef;
        this.prepTime = this.recipe.prepTime;
        this.ingredients = this.recipe.ingredients;
        this.directions = this.recipe.directions;
        this.imageUrl = this.recipe.imageUrl;
      });
    },
    updateRecipe: function() {
      var params = {
        title: this.title,
        chef: this.chef,
        prep_time: this.prepTime,
        ingredients: this.ingredients,
        directions: this.directions,
        image_url: this.imageUrl,
      };
      axios
        .patch("/api/recipes/" + this.$route.params.id, params)
        .then(response => {
          console.log(response);
          this.$router.push("/recipes");
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>
