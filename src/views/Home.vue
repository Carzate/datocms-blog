<template>
  <div v-if="data">
    <v-container>
    <div v-for="(blog, index) in data.allPosts" :key="index" class="mb-12">
      <router-link class="titulo" :to="`/post/${blog.slug}`">{{blog.title}}</router-link>
      <p class="mt-4 mb-4">{{blog.excerpt}}</p>
      <datocms-image :data="blog.coverimage.responsiveImage"/>
    </div> 
    </v-container>
    </div>
</template>

<script>
import Header from '@/components/Header.vue'
import { request } from "@/datocms";
import { Image } from "vue-datocms";

const HOMEPAGE_QUERY = `query MyQuery {
  allPosts {
    title
    slug
    id
    excerpt
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
    "datocms-image": Image,
    Header
  },
  name: "Home",
  data: () => ({
    blog: null,
    data: null,
    error: null,
    loading: true,
  }),
  async created() {
    try {
      this.data = await request({
        query: HOMEPAGE_QUERY,
        variables: {
          limit: 10
        }
      });
    } catch (e) {
      this.error = e;
    }
    this.loading = false;
  }
};
</script>

<style>
  .titulo {
    font-size: 40px;
    color: pink !important; 
    text-decoration: none;
  }
</style>