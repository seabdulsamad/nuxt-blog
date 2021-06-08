<template>
  <v-container grid-list-lg>
    <v-layout row wrap fill-height>
      <v-flex sm4 v-for="post in posts" :key="post.id">
        <nuxt-link
          :to="{
            name: 'posts-id',
            params: { id: _.kebabCase(post.title) },
            query: { postId: post.id },
          }"
          class="text-decoration-none"
        >
          <v-card class="full-height">
            <v-card-title>{{ post.title }}</v-card-title>
            <v-card-text>
              <v-icon small left>mdi-account</v-icon>{{ post.user.name }}
              <v-icon small left right>mdi-calendar</v-icon
              >{{ post.created_at }}
              <div class="pt-2">{{ post.description }}</div>
            </v-card-text>
          </v-card>
        </nuxt-link>
      </v-flex>
      <!-- Add New post action -->
      <v-flex>
        <v-btn
          fab
          color="primary"
          class="floating-new-post-btn"
          :to="{
            name: 'posts-actions-id',
            params: { actions: 'create', id: 'new' },
          }"
        >
          <v-icon>mdi-plus</v-icon>
        </v-btn>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    posts: [],
  }),
  created() {
    this.fetchPosts();
  },
  methods: {
    fetchPosts() {
      this.$axios.get("/posts").then((res) => (this.posts = res.data));
    },
  },
};
</script>
<style>
.floating-new-post-btn {
  position: fixed !important;
  right: 20px;
  bottom: 20px;
}
</style>
