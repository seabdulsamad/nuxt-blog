<template>
  <v-app dark>
    <v-main>
      <v-container grid-list-lg>
        <v-layout row wrap>
          <v-flex sm9 class="mx-auto">
            <v-layout class="my-7">
              <v-flex class="logo-flex-wrap">
                <nuxt-link to="/" class="text-decoration-none float-left">
                  <!-- <v-img
                    :src="`${$appURL}images/logo/CPlogo.png`"
                    :width="150"
                  /> -->
                </nuxt-link>
              </v-flex>
            </v-layout>
            <v-layout row wrap>
              <v-flex sm6>
                <nuxt />
              </v-flex>
            </v-layout>
          </v-flex>
        </v-layout>
      </v-container>
    </v-main>
  </v-app>
</template>
<script>
export default {
  computed: {
    loggedIn() {
      return this.$auth.loggedIn;
    },
  },
  watch: {
    loggedIn() {
      if (this.loggedIn) this.$router.replace({ name: "posts-index" });
    },
  },
};
</script>