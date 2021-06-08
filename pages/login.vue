<template>
  <v-form ref="form" v-on:submit.prevent>
    <!-- Login Form -->
    <v-layout column>
      <v-flex>
        <v-autocomplete
          label="Email"
          name="loginUserEmail"
          v-model="username"
          :items="users"
          item-text="email"
          item-value="email"
          solo
          hide-details
          flat
          outlined
          dense
          @keyup.enter="getAuth"
          clearable
          :rules="[(v) => !!v || 'Required']"
          :inputProps="{ autocomplete: false }"
        />
      </v-flex>
      <v-flex>
        <v-text-field
          v-model="password"
          :type="!viewPassword ? 'password' : 'text'"
          label="Password"
          solo
          hide-details
          flat
          outlined
          dense
          :rules="[(v) => !!v || 'Required']"
          :append-icon="viewPassword ? 'mdi-eye-off' : 'mdi-eye'"
          @keyup.enter="getAuth"
          @click:append="viewPassword = !viewPassword"
        />
      </v-flex>
      <v-flex>
        <v-btn
          :loading="loading"
          color="primary"
          block
          :height="40"
          @click="getAuth"
          >Login</v-btn
        >
      </v-flex>
    </v-layout>
  </v-form>
</template>
<script>
export default {
  layout: "auth",
  data: () => ({
    username: null,
    password: "123456",
    viewPassword: false,
    users: [],
  }),
  created() {
    this.fetchUsers();
    this.$auth.$storage.setState("loginLoading", false);
  },
  computed: {
    loading() {
      return this.$auth.$storage.getState("loginLoading");
    },
    loggedIn() {
      return this.$auth.loggedIn;
    },
  },
  methods: {
    fetchUsers() {
      this.$axios.get("/public/users").then((res) => (this.users = res.data));
    },
    async getAuth() {
      if (this.$refs.form.validate()) {
        this.$auth.$storage.setState("loginLoading", true);
        try {
          await this.$auth.loginWith("local", {
            params: {
              email: this.username,
              password: this.password,
            },
          });
        } catch (error) {
          this.$auth.$storage.setState("loginLoading", false);
        }
      }
    },
  },
  watch: {
    loggedIn() {
      if (this.loggedIn) this.$router.replace({ name: "posts-index" });
    },
  },
};
</script>