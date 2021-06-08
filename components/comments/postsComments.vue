<template>
  <v-layout>
    <v-list>
      <v-list-item v-for="comment in comments" :key="comment.id">
        <v-list-item-avatar class="grey lighten-4">AB</v-list-item-avatar>
        <v-list-item-content>
          <div>{{ comment.user.name }}</div>
          <div class="caption">
            <v-icon small left>mdi-calendar</v-icon>{{ comment.created_at }}
          </div>
          <p class="caption">{{ comment.text }}</p>
        </v-list-item-content>
      </v-list-item>
    </v-list>
  </v-layout>
</template>
<script>
export default {
  props: {
    postId: { type: Number, required: true },
    reload: { type: Boolean },
  },
  data: () => ({
    comments: [],
  }),
  created() {
    this.fetchPostComments();
  },
  methods: {
    fetchPostComments() {
      this.$axios
        .get(`posts/${this.postId}/comments`)
        .then((res) => (this.comments = res.data))
        .finally(() => {
          this.$emit("onReloaded", true);
        });
    },
  },
  watch: {
    reload() {
      if (this.reload) this.fetchPostComments();
    },
  },
};
</script>