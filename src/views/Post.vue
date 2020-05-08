<template>
  <div class="home" v-if="data">
    <div v-for="(post, index) in data.allPosts" :key="index">
        <p>{{post.id}}</p>
        <p>{{post.slug}}</p>
        <p>{{post.title}}</p>
        <p>{{post.body}}</p>
       <datocms-image :data="post.coverimage.responsiveImage" />
    </div>
    </div>
</template>

<script>
import { request } from "@/datocms";
import { Image } from "vue-datocms";

const POST_QUERY = `query MyQuery($slug: String!){
  allPosts(filter: { slug: { eq: $slug } }){
    title
    slug
    id
    body
    coverimage {
      responsiveImage{
        aspectRatio
        width
        sizes
        srcSet
        src
      }
    }
  }
}`;

export default {
  components: {
    "datocms-image": Image
  },
  name: "Home",
  data: () => ({
    uuid: null,
    blog: null,
    data: null,
    error: null,
    loading: true,
  }),
  async created() {
    this.uuid = this.$route.params.id 
    try {
      this.data = await request({
        query: POST_QUERY,
        variables: {
          limit: 10,
          slug: this.uuid
        }
      });
    } catch (e) {
      this.error = e;
    }
    this.loading = false;
  }
};
</script>