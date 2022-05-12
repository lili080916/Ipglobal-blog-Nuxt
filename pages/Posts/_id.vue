<template>
  <div>
      <div>
        <b-card :title="post.title" :sub-title="user.name">
          <b-card-text>
            {{post.body}}
          </b-card-text>

          <b-card-text><strong> User data: </strong></b-card-text>
          <b-card-text>
            <b-card-text-body>
              <b-card-text-body-item>
                <b-card-text-body-item-label>Name</b-card-text-body-item-label>
                <b-card-text-body-item-content>{{user.name}}</b-card-text-body-item-content>
              </b-card-text-body-item>
              <br>
              <b-card-text-body-item>
                <b-card-text-body-item-label>Email</b-card-text-body-item-label>
                <b-card-text-body-item-content>{{user.email}}</b-card-text-body-item-content>
              </b-card-text-body-item>
              <br>
              <b-card-text-body-item>
                <b-card-text-body-item-label>Phone</b-card-text-body-item-label>
                <b-card-text-body-item-content>{{user.phone}}</b-card-text-body-item-content>
              </b-card-text-body-item>
              <br>
              <b-card-text-body-item>
                <b-card-text-body-item-label>website</b-card-text-body-item-label>
                <b-card-text-body-item-content>{{user.website}}</b-card-text-body-item-content>
              </b-card-text-body-item>
              <br>

              <b-card-text-body-item>
                <b-card-text-body-item-label>Address</b-card-text-body-item-label>
                <b-card-text-body-item-content>
                  <p>street: {{address.street}}</p>
                  <p>suite: {{address.suite}}</p>
                  <p>city: {{address.city}}</p>
                  <p>zipcode": {{address.zipcode}}</p>
                </b-card-text-body-item-content>
              </b-card-text-body-item>
              <br>
              <b-card-text-body-item>
                <b-card-text-body-item-label>company</b-card-text-body-item-label>
                <b-card-text-body-item-content>
                  <p>name: {{company.name}}</p>
                  <p>catchPhrase: {{company.catchPhrase}}</p>
                  <p>bs: {{company.bs}}</p>
                </b-card-text-body-item-content>
              </b-card-text-body-item>

            </b-card-text-body>
          </b-card-text>

        </b-card>
      </div>
  </div>
</template>

<script>
export default {
    name: 'PostDetails',
    layout: 'header',
    async asyncData({params}) {

        return {
          id: params.id
        };
    },
    data() {
        return {
            user: {},
            address: {},
            company: {},
            post: {}
        }
    },
    mounted() {
      this.fetchPost();
    },
    methods: {
      async fetchPost() {
        const {id} = this;

        const post = await this.$axios.$get(api_posts + id);
        this.post = post.data;
        this.user = this.post.user;
        this.address = JSON.parse(this.user.address);
        this.company = JSON.parse(this.user.company);
      },

      fixedData(data) {
        return `
          street: ${data.street}
          suite: ${data.suite}
          city": ${data.city}
          zipcode": ${data.zipcode}
        `;
      }
    }
}
</script>

<style>

</style>
