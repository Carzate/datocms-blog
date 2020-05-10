<template>
  <div class="home" v-if="data">
        <v-container>
            <div v-for="(post, index) in data.allPosts" :key="index">
                <p>id: {{post.id}}</p>
                <p>slug: {{post.slug}}</p>
                <h1 class="text-center display-3 mb-12" style="color: yellow">{{post.title}}</h1>
                <div v-html="post.body"></div>
                <div class="d-flex justify-center">
                    <datocms-image :data="post.coverimage.responsiveImage" />
                </div>
            </div>
        </v-container>
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
    body(markdown: true)
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