<template>
  <div class="home">
    <section class="recipe-collection">
      <div class="recipe" v-for="recipe in recipes" :key="recipe.id">
        <h1>{{recipe.title}}</h1>
        <p>Uploaded by {{recipe.uploadedBy}}</p>
        <img :src="recipe.path" />
        <h1>Ingredients:</h1>
        <p>{{recipe.ingredients}}</p>
        <h1>Directions:</h1>
        <p>{{recipe.directions}}</p>
        <button @click="favoriteRecipe(recipe)">Favorite Recipe!</button>
        <button @click="deleteRecipe(recipe)">Delete Recipe</button>
      </div>
    </section>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from "axios";

export default {
  name: "Home",
  data() {
    return {
      recipes: []
    };
  },
  created() {
    this.getRecipes();
  },
  methods: {
    async getRecipes() {
      try {
        let response = await axios.get("/api/recipes");
        this.recipes = response.data;
        return true;
      } catch (error) {
        //console.log(error);
      }
    },
    async deleteRecipe(recipe) {
      try {
        //console.log(recipe);
        await axios.delete("/api/recipes/" + recipe._id);
        this.getRecipes();
        return true;
      } catch (error) {
        //console.log(error);
      }
    },
    async favoriteRecipe(recipe) {
      try {
        await axios.put("/api/recipes/" + recipe._id, {
          saved: true
        });
        this.getRecipes();
        alert("Favorited " + recipe.title);
        return true;
      } catch (error) {
        //console.log(error);
      }
    }
  }
};
</script>

<style scoped>
p {
  white-space: pre-line;
}

img {
  width: 200px;
  margin: 10px;
}

.recipe {
  margin: 10px;
  padding: 15px;
  border-radius: 15px;
  background-color: rgb(238, 238, 238);
}
</style>