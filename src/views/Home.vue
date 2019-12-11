<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div v-for="recipe in recipes">
      <h3>{{ recipe.title }}</h3>
      <img v-bind:src="recipe.image_url" alt="" />
      <p>Ingredients: {{ recipe.ingredients }}</p>
      <p>Directions: {{ recipe.directions }}</p>
    </div>

    <h1>New recipe</h1>
    <button v-on:click="createRecipe()">Create</button>
  </div>
</template>

<style>
img {
  width: 300px;
}
</style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      message: "Welcome to Vue.js!!!",
      recipes: []
    };
  },
  created: function() {
    axios.get("/api/recipes").then(response => {
      this.recipes = response.data;
      console.log(this.recipes);
    });
  },
  methods: {
    createRecipe: function() {
      console.log("Create the recipe...");
      var params = {
        input_title: "Another Example title",
        input_chef: "Example chef",
        input_ingredients: "Example ingredients",
        input_directions: "Example directions",
        input_prep_time: 12
      };
      axios
        .post("/api/recipes", params)
        .then(response => {
          var recipe = response.data;
          this.recipes.push(recipe);
          console.log("Success", recipe);
        })
        .catch(error => console.log(error.response));
    }
  }
};
</script>
