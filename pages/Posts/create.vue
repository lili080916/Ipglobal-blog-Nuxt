<template>
  <div>
      <div>
        <b-card title="Create" sub-title="Post" style="margin-top: 70px;">
          <form method="post" action
          @submit.prevent="handleSumbit()"
          >
            <b-form-group label="Title" label-for="title">
              <b-form-input id="title" v-model="post.title" required placeholder="Title"></b-form-input>
            </b-form-group>
            <b-form-group label="Body" label-for="body">
              <b-form-textarea id="body" v-model="post.body" required placeholder="Body"></b-form-textarea>
            </b-form-group>
            <b-button type="submit" variant="primary">Add</b-button>
          </form>

        </b-card>
      </div>
  </div>
</template>

<script>
export default {
    name: 'PostCraetePage',
    layout: 'header',
    data() {
        return {
            user_id: 3,
            post: {
              title: '',
              body: ''
            }
        }
    },

    methods: {
      async handleSumbit() {
        let params =  {
            'user_id': this.user_id,
            'title': this.post.title,
            'body': this.post.body
          }

        await this.$axios.$post(api_posts, params);

        this.$router.push('/');
      },
    }
}
</script>

<style>

</style>
