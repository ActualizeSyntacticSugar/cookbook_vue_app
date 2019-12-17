<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h3>{{ recipe.title }}</h3>
    <img v-bind:src="recipe.image_url" alt="" />
    <p>Ingredients: {{ recipe.ingredients }}</p>
    <p>Directions: {{ recipe.directions }}</p>
    <p>Prep time: {{ recipe.prep_time }}</p>
    <a href="/recipes">All recipes</a>
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
      recipe: {}
    };
  },
  created: function() {
    axios.get("/api/recipes/" + this.$route.params.id).then(response => {
      this.recipe = response.data;
      console.log(this.recipe);
    });
  },
  methods: {}
};
</script>
