<template>
  <v-textarea
    rows="1"
    auto-grow
    label="Write your comment here...."
    append-icon="mdi-send"
    @click:append="onSaveComment"
    v-model="params.text"
    :loading="loading"
  />
</template>
<script>
export default {
  props: { postId: { type: Number, required: true } },
  data: () => ({ params: { text: null, post_id: null }, loading: false }),
  created() {
    this.params.post_id = this.postId;
  },
  methods: {
    onSaveComment() {
      if (_.size(this.params.text)) {
        this.loading = true;
        this.$axios
          .post(`/comments`, this.params)
          .then((res) => (this.comments = res.data))
          .finally(() => {
            this.loading = false;
            this.params.text = null;
            this.$emit("onPostedComment");
          });
      }
    },
  },
};
</script>