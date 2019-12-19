<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div>
      Search recipes:
      <input type="text" v-model="titleFilter" list="titles" />
      <datalist id="titles">
        <option v-for="recipe in recipes">{{ recipe.title }}</option>
      </datalist>
    </div>
    <div v-for="recipe in filterBy(recipes, titleFilter, 'title', 'ingredients')">
      <h3>{{ recipe.title }}</h3>
      <img v-bind:src="recipe.image_url" alt="" />
      <p>Ingredients: {{ recipe.ingredients }}</p>
      <router-link v-bind:to="`/recipes/${recipe.id}`">Home</router-link>
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
import Vue2Filters from "vue2-filters";

export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      message: "Welcome to Vue.js!!!",
      recipes: [],
      titleFilter: ""
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
