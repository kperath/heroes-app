<template>
  <v-main class>
    <Notification
      :success="success"
      :error="error"
      :successMessage="successMessage"
      :errorMessage="errorMessage"
    />
    <v-card dark class="mx-auto elevation-12" max-width="500">
      <v-card-title class="teal--text text--accent-1">
        <span class="headline">Login</span>
      </v-card-title>
      <v-card-text>
        <v-form ref="form">
          <v-row>
            <v-col cols="12">
              <v-text-field
                label="Username"
                counter
                maxlength="15"
                :rules="validationRules"
                required
                v-model="username"
              ></v-text-field>
            </v-col>
            <v-col cols="12">
              <v-text-field
                label="Password"
                type="password"
                :rules="validationRules"
                required
                v-model="password"
              ></v-text-field>
            </v-col>
          </v-row>
          <router-link
            :to="{name: 'Register'}"
            class="light-blue--text text--lighten-3"
          >Not an existing user? Register here!</router-link>
        </v-form>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="teal accent-1" text @click="login">Sign In</v-btn>
      </v-card-actions>
    </v-card>
  </v-main>
</template>

<script>
import { notifyMixin } from "../mixins/notifyMixin";
import Notification from "../components/Notification";

export default {
  name: "Login",
  mixins: [notifyMixin],
  components: { Notification },
  data: () => ({
    PORT: 3002,
    username: "",
    password: "",
    validationRules: [(v) => v != "" || "Required"],
  }),
  methods: {
    login() {
      if (this.$refs.form.validate()) {
        // creating user object
        const user = {};
        user["username"] = this.username;
        user["password"] = this.password;

        // login user
        let PORT = this.PORT ? this.PORT : 3002;
        fetch(`http://localhost:${PORT}/api/user/login`, {
          method: "POST", // might change to POST for token creation
          mode: "cors",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify(user),
        })
          .then((resp) => {
            return resp.json();
          })
          .then((data) => {
            // store jwt using vuex
            this.$store.commit("setJWT", data);
            this.$router.replace({ name: "Home" });
          })
          .catch((err) => {
            console.log(err);
            this.errorNotify(new Error("Failed to Login"));
          });
        this.$refs.form.reset();
      }
    },
  },
};
</script>
