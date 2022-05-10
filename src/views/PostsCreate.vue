<script>
import axios from "axios";

export default {
  data: function () {
    return {
      newPostParams: {},
      errors: [],
      status: "",
    };
  },
  methods: {
    createPost: function () {
      console.log("Making a new post!");

      axios
        .post("http://localhost:3000/posts.json", this.newPostParams)
        .then((response) => {
          console.log("Success", response.data);
          this.post.push(response.data);
        })
        .catch((error) => {
          console.log(error.response.status, error.response.statusText), (this.status = error.response.status);
        });
    },
  },
};
</script>

<template>
  <div class="post-new">
    <img v-if="status" v-bind:src="`https://http.cat/${status}`" alt="" />
    <form v-on:submit.prevent="createPost()">
      <h1>
        This is the place to make a
        <span class="text-info">new</span>
        post!
      </h1>

      <div>
        <div class="form-group">
          title:
          <input type="text" v-model="newRecipeParams.title" />
        </div>
        <div class="form-group">
          text:
          <input type="text" v-model="newRecipeParams.chef" />
        </div>
      </div>
      <input type="submit" value="create" />
    </form>
  </div>
</template>
