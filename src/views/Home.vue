<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div v-for="recipe in recipes">
      <h3>{{ recipe.title }}</h3>
      <img v-bind:src="recipe.image_url" alt="" />
      <p>Ingredients: {{ recipe.ingredients }}</p>
      <p>Directions: {{ recipe.directions }}</p>
    </div>
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
  methods: {}
};
</script>
