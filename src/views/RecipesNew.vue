<template>
  <div class="signup">
    <div class="container">
      <form v-on:submit.prevent="submit()">
        <h1>New Recipe</h1>
        <ul>
          <li class="text-danger" v-for="error in errors">{{ error }}</li>
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
          <label>Ingredients:</label>
          <input type="text" class="form-control" v-model="ingredients" />
        </div>
        <div class="form-group">
          <label>Directions:</label>
          <input type="text" class="form-control" v-model="directions" />
        </div>
        <div class="form-group">
          <label>Prep time:</label>
          <input type="text" class="form-control" v-model="prep_time" />
        </div>
        <div class="form-group">
          <label>Image url:</label>
          <input type="file" class="form-control" v-on:change="setFile($event)" ref="fileInput" />
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
      title: "",
      chef: "",
      ingredients: "",
      directions: "",
      prep_time: "",
      image_file: "",
      errors: []
    };
  },
  methods: {
    setFile: function(event) {
      if (event.target.files.length > 0) {
        this.image_file = event.target.files[0];
      }
    },
    submit: function() {
      var formData = new FormData();
      formData.append("input_title", this.title);
      formData.append("input_chef", this.chef);
      formData.append("input_ingredients", this.ingredients);
      formData.append("input_directions", this.directions);
      formData.append("input_prep_time", this.prep_time);
      formData.append("input_image_file", this.image_file);
      // var params = {
      //   input_title: this.title,
      //   input_chef: this.chef,
      //   input_ingredients: this.ingredients,
      //   input_directions: this.directions,
      //   input_prep_time: this.prep_time,
      //   input_image_url: this.image_url
      // };
      axios
        .post("/api/recipes", formData)
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
