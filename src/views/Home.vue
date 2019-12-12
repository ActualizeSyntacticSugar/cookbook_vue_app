<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div v-for="recipe in recipes">
      <h3>{{ recipe.title }}</h3>
      <button v-on:click="showRecipe(recipe)">More info</button>
      <div v-if="recipe === currentRecipe">
        <img v-bind:src="recipe.image_url" alt="" />
        <p>Ingredients: {{ recipe.ingredients }}</p>
        <p>Directions: {{ recipe.directions }}</p>
        <h4>Edit Recipe</h4>
        <div>
          Title:
          <input type="text" v-model="recipe.title" />
          Chef:
          <input type="text" v-model="recipe.chef" />
          Ingredients:
          <input type="text" v-model="recipe.ingredients" />
          Directions:
          <input type="text" v-model="recipe.directions" />
          Prep time:
          <input type="text" v-model="recipe.prep_time" />
          Image url:
          <input type="text" v-model="recipe.image_url" />
          <button v-on:click="updateRecipe(recipe)">Update</button>
        </div>
      </div>
    </div>

    <h1>New recipe</h1>
    Title:
    <input type="text" v-model="newRecipeTitle" />
    Chef:
    <input type="text" v-model="newRecipeChef" />
    Ingredients:
    <input type="text" v-model="newRecipeIngredients" />
    Directions:
    <input type="text" v-model="newRecipeDirections" />
    Prep Time:
    <input type="text" v-model="newRecipePrepTime" />
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
      recipes: [],
      currentRecipe: {},
      newRecipeTitle: "",
      newRecipeChef: "",
      newRecipeIngredients: "",
      newRecipeDirections: "",
      newRecipePrepTime: ""
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
        input_title: this.newRecipeTitle,
        input_chef: this.newRecipeChef,
        input_ingredients: this.newRecipeIngredients,
        input_directions: this.newRecipeDirections,
        input_prep_time: this.newRecipePrepTime
      };
      axios
        .post("/api/recipes", params)
        .then(response => {
          var recipe = response.data;
          this.recipes.push(recipe);
          this.newRecipeTitle = "";
          this.newRecipeChef = "";
          this.newRecipeIngredients = "";
          this.newRecipeDirections = "";
          this.newRecipePrepTime = "";
          console.log("Success", recipe);
        })
        .catch(error => console.log(error.response));
    },
    showRecipe: function(recipe) {
      if (this.currentRecipe === recipe) {
        this.currentRecipe = {};
      } else {
        this.currentRecipe = recipe;
      }
    },
    updateRecipe: function(recipe) {
      console.log("updateRecipe...", recipe);
      var params = {
        input_title: recipe.title,
        input_chef: recipe.chef,
        input_prep_time: recipe.prep_time,
        input_ingredients: recipe.ingredients,
        input_directions: recipe.directions,
        input_image_url: recipe.image_url
      };
      axios.patch("/api/recipes/" + recipe.id, params).then(response => {
        console.log(response);
      });
    }
  }
};
</script>
