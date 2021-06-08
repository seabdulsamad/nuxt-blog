<template>
  <v-app>
    <v-app-bar fixed app color="primary" dark>
      <v-toolbar-title v-text="title" />
      <v-spacer />
      <v-flex shrink v-if="$auth.loggedIn">
        {{ $auth.user.name }}&nbsp;<small>({{ $auth.user.email }})</small>
      </v-flex>
      <v-btn
        class="ml-3"
        small
        depressed
        color="secondary"
        v-if="$auth.loggedIn"
        @click="onLogout()"
      >
        Logout
      </v-btn>
    </v-app-bar>
    <v-main>
      <v-container>
        <nuxt />
      </v-container>
    </v-main>
    <v-footer :absolute="!fixed" app>
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: "mdi-apps",
          title: "Welcome",
          to: "/",
        },
        {
          icon: "mdi-chart-bubble",
          title: "Inspire",
          to: "/inspire",
        },
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: "Nuxt Blog",
    };
  },
  methods: {
    onLogout() {
      this.$auth.logout();
      this.$router.replace({ name: "login" });
    },
  },
};
</script>
