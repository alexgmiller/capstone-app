<script>
import axios from "axios";

export default {
  data: function () {
    return {
      newUserParams: { username: "", password: "", password_confirmation: "" },
      errors: [],
    };
  },
  methods: {
    submit: function () {
      axios
        .post("/users", this.newUserParams)
        .then((response) => {
          console.log(response.data);
          this.$router.push("/login");
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>

<template>
  <div class="signup">
    <form v-on:submit.prevent="submit()">
      <h1>Signup</h1>
      <ul>
        <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>
      <div>
        <label>Username:</label>
        <input type="text" v-model="newUserParams.username" />
        <small>{{ 20 - newUserParams.username.length }} characters remaining</small>
      </div>
      <div>
        <label>Email:</label>
        <input type="email" v-model="newUserParams.email" />
      </div>
      <div>
        <label>Password:</label>
        <input type="password" v-model="newUserParams.password" />
        <small v-if="newUserParams.password.length > 0 && newUserParams.password.length < 6" class="text-danger">
          Must be at least 6 characters
        </small>
        <small v-if="newUserParams.password.length > 20" class="text-danger">
          Password cannot exceed 20 characters
        </small>
      </div>
      <div>
        <label>Password confirmation:</label>
        <input type="password" v-model="newUserParams.password_confirmation" />
        <small v-if="newUserParams.password_confirmation !== newUserParams.password" class="text-danger">
          Password confirmations does not match password
        </small>
      </div>
      <input type="submit" value="Submit" />
    </form>
  </div>
</template>
