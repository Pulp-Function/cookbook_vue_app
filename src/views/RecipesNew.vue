<template>
  <div class="signup">
    <img v-if="status" v-bind:src="`https://http.cat/${status}`" v-on:click="status = ''" alt="" />
    <form v-on:submit.prevent="createRecipe()">
      <h1>New Recipe</h1>
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
        <input type="file" class="form-control" v-on:change="setFile($event)" ref="fileInput" />
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
      title: "",
      chef: "",
      prepTime: "",
      ingredients: "",
      directions: "",
      image: "",
      status: "",
      errors: [],
    };
  },
  methods: {
    setFile: function(event) {
      if (event.target.files.length > 0) {
        this.image = event.target.files[0];
      }
    },
    createRecipe: function() {
      // var params = {
      //   title: this.title,
      //   chef: this.chef,
      //   prep_time: this.prepTime,
      //   ingredients: this.ingredients,
      //   directions: this.directions,
      //   image_url: this.imageUrl,
      // };

      var params = new FormData();
      params.append("title", this.title);
      params.append("chef", this.chef);
      params.append("prep_time", this.prepTime);
      params.append("ingredients", this.ingredients);
      params.append("directions", this.directions);
      params.append("image", this.image);

      axios
        .post("/api/recipes", params)
        .then(response => {
          console.log(response);
          this.$router.push("/recipes");
        })
        .catch(error => {
          this.errors = error.response.data.errors;
          this.status = error.response.status;
        });
    },
  },
};
</script>
