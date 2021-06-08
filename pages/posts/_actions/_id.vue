<template>
  <v-form ref="form" v-on:submit.prevent>
    <v-container grid-list-lg>
      <v-layout row wrap>
        <v-flex sm6>
          <v-text-field
            v-model="params.title"
            label="Title"
            clearable
            :rules="[(v) => !!v || 'The specified field is required']"
          ></v-text-field>
        </v-flex>
        <v-flex sm12>
          <v-textarea
            label="Description"
            clearable
            v-model="params.description"
            auto-grow
            :rules="[(v) => !!v || 'The specified field is required']"
          ></v-textarea>
        </v-flex>
        <v-flex>
          <v-btn color="primary" :loading="loading" @click="onSavePost">
            Save & Publish &rarr;
          </v-btn>
          <v-btn text color="secondary" :to="{ name: 'posts' }">Cancel</v-btn>
        </v-flex>
      </v-layout>
    </v-container>
  </v-form>
</template>
<script>
export default {
  data: () => ({
    params: { title: null, description: null },
    loading: false,
  }),
  methods: {
    onSavePost() {
      if (this.$refs.form.validate()) {
        this.$axios
          .post("/posts", this.params)
          .then(() => {
            this.$refs.form.reset();
          })
          .finally(() => {
            this.loading = false;
          });
      }
    },
  },
};
</script>