<template>
  <div>
    <div class="header-table">

    </div>
    <Table
        :items="posts"
        :fields="fields"
        :sort_by="'status'"
        :sort_desc="true"
        :per_page="10"
        show_pagination
        :loading="loading"
    >
        <template v-slot:title="data">
            {{ data.value }}
        </template>

        <template v-slot:body="data">
            {{ data.value }}
        </template>

        <template v-slot:link="data">
            <div class="w-100 h-100 d-flex justify-content-center align-items-center">
                <nuxt-link :to="`/posts/${data.value}`">
                    Details
                </nuxt-link>
            </div>
        </template>
    </Table>
  </div>
</template>

<script>
export default {
  name: 'PostPage',
  data() {
    return {
      posts: [],
      fields() {
          return [
              { key: 'title', label: 'Title', sortable: true},
              { key: 'body', label: 'body', sortable: true},
              { key: 'link', label: 'Actions', sortable: false}
          ];
      },
      loading: true,
    }
  },

  mounted() {
    this.fetchPosts();
  },

  methods: {
    async fetchPosts() {
      let response  = await this.$axios.$get(api_posts);

      response.data.forEach(post => {
        this.posts.push({
          title: post.title,
          body: post.body,
          link: post.id
        });
      });

      this.loading = false;
    }
  }
}
</script>

<style lang="scss" scoped>
    .empty-results {
        margin: 0 auto;

        width: 374px;
        height: 174px;

        background: white;
        border-radius: 10px;

        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: space-evenly;

        text-align: center;

        box-shadow: 0px 0px 20px 0px #00000080;

        margin-top: 9rem;
    }

    .disabled {
        color: #9CA5AB;
    }
</style>
