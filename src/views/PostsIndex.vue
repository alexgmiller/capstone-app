<script>
import axios from "axios";
import moment from "moment";

export default {
  data: function () {
    return {
      recipes: [],
      currentPost: {},
      titleFilter: "",
    };
  },

  created: function () {
    axios.get("http://localhost:3000/posts.json").then((response) => {
      this.posts = response.data;
      console.log("All posts", this.posts);
    });
  },
  methods: {
    relativeDate: function (date) {
      return moment(date).fromNow();
    },
    filterPosts: function () {
      return this.posts.filter((post) => {
        var lowerTitle = post.title.toLowerCase();
        var lowerTitleFilter = this.titleFilter.toLowerCase();
        return lowerTitle.includes(lowerTitleFilter);
      });
    },
    // orderBy: function (recipes) {
    //   return recipes.sort((a, b) => {
    //     if (a.title < b) {
    //       return -1;
    //     } else if (a.title > b) {
    //       return 1;
    //     } else {
    //       return 0;
    //     }
    //   });
    // },
    sortedArray() {
      let sortedPosts = this.recipes;

      sortedPosts = sortedPosts.sort((a, b) => {
        let fa = a.title.toLowerCase(),
          fb = b.title.toLowerCase();
        if (fa < fb) {
          return -1;
        }
        if (fa > fb) {
          return 1;
        }
        return 0;
      });
      return sortedPosts;
    },
  },
  computed: {},
};
</script>

<template>
  <h1>HERE ARE posts</h1>
  <div class="row">
    <input v-model="titleFilter" type="text" list="titles" />
    <datalist id="titles">
      <option v-for="recipe in recipes" v-bind:key="recipe.id">{{ recipe.title }}</option>
    </datalist>
    <button v-on:click="sortedArray()">Sort by Title</button>
    <!-- <div v-for="recipe in orderBy(recipes)" :key="recipe.id">
      {{ recipe }}
      <br />
      <br />
    </div> -->
    <button>Sort by prep time</button>
    <div
      v-for="recipe in filterRecipes()"
      v-on:click="currentRecipe = recipe"
      v-bind:key="recipe.id"
      class="col-sm-4 mb-4"
    >
      <div class="card">
        <div v-bind:class="{ selected: recipe === currentRecipe }">
          <img :src="recipe.image_url" v-bind:alt="recipe.title" class="card-img-top" />
          <div class="card-body">
            <h5 class="card-title">Title: {{ recipe.title }}</h5>
            <p class="card-text">Text: {{ recipe.chef }}</p>
            <p class="card-text">Created: {{ relativeDate(recipe.created_at) }}</p>
            <a v-bind:href="`/recipes/${recipe.id}`" class="btn btn-primary">More info</a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
.card img {
  height: 300px;
  object-fit: cover;
}

.selected {
  color: greenyellow;
  background-color: purple;
  transition: background-color 1s ease;
}
</style>
