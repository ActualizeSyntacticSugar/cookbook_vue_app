<template>
  <div class="signup">
    <div class="container">
      <form v-on:submit.prevent="submit()">
        <h1>Edit Recipe</h1>
        <ul>
          <li class="text-danger" v-for="error in errors">{{ error }}</li>
        </ul>
        <div class="form-group">
          <label>Title:</label>
          <input type="text" class="form-control" v-model="recipe.title" />
        </div>
        <div class="form-group">
          <label>Chef:</label>
          <input type="text" class="form-control" v-model="recipe.chef" />
        </div>
        <div class="form-group">
          <label>Ingredients:</label>
          <input type="text" class="form-control" v-model="recipe.ingredients" />
        </div>
        <div class="form-group">
          <label>Directions:</label>
          <input type="text" class="form-control" v-model="recipe.directions" />
        </div>
        <div class="form-group">
          <label>Prep time:</label>
          <input type="text" class="form-control" v-model="recipe.prep_time" />
        </div>
        <input type="submit" class="btn btn-primary" value="Submit" />
      </form>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      recipe: {},
      errors: []
    };
  },
  created: function() {
    axios.get("/api/recipes/" + this.$route.params.id).then(response => {
      this.recipe = response.data;
      console.log(this.recipe);
    });
  },
  methods: {
    submit: function() {
      var params = {
        input_title: this.recipe.title,
        input_chef: this.recipe.chef,
        input_ingredients: this.recipe.ingredients,
        input_directions: this.recipe.directions,
        input_prep_time: this.recipe.prep_time
      };
      axios
        .patch("/api/recipes/" + this.$route.params.id, params)
        .then(response => {
          this.$router.push("/recipes");
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    }
  }
};
</script>
