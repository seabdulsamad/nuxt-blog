<template>
  <v-container grid-list-lg v-if="post">
    <v-flex>
      <h1 class="headline pb-2">{{ post.title }}</h1>
      <template v-if="post.user">
        <v-icon small left>mdi-account</v-icon>{{ post.user.name }}
      </template>
      <v-icon small left>mdi-calendar</v-icon>{{ post.created_at }}
    </v-flex>
    <p class="py-3">{{ post.description }}</p>
    <h3 class="text-lg">Comments</h3>
    <v-flex>
      <post-comments
        :postId="$route.query.postId"
        :reload="reloadComments"
        @onReloaded="reloadComments = false"
      />
    </v-flex>
    <v-flex>
      <save-comment
        :postId="$route.query.postId"
        @onPostedComment="reloadComments = true"
      />
    </v-flex>
  </v-container>
</template>
<script>
import PostComments from "@/components/comments/postsComments";
import SaveComment from "@/components/comments/saveComment";
export default {
  components: { "post-comments": PostComments, "save-comment": SaveComment },
  data: () => ({ post: null, reloadComments: false }),
  created() {
    this.fetchPostById();
  },
  methods: {
    fetchPostById() {
      this.$axios
        .get(`posts/${this.$route.query.postId}`)
        .then((res) => (this.post = res.data));
    },
  },
};
</script>