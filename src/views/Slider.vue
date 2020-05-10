<template>
  <div v-if="data">
    <v-container>
    <agile :slidesToShow="3" centerMode >
        <div v-for="(post, index) in data.allPosts" :key="index" class="slide" @click="$router.push({ path: `/post/${post.slug}` })" style="margin-right: 30px">
            <datocms-image :data="post.coverimage.responsiveImage"/>
            <h3>{{post.title}}</h3>
        </div>
    </agile>
    <!-- <VueSlickCarousel :v-bind="settings">
      <div><img src="https://i.picsum.photos/id/237/200/300.jpg" alt=""></div>
    </VueSlickCarousel> -->
    </v-container>
    </div>
</template>

<script>
import Header from '@/components/Header.vue'
import { request } from "@/datocms";
import { Image } from "vue-datocms";
import { VueAgile } from 'vue-agile';
import VueSlickCarousel from 'vue-slick-carousel'

export default {
  components: {
    "datocms-image": Image,
    Header,
    agile: VueAgile,
    VueSlickCarousel
  },
  name: "Home",
  data: () => ({
    settings: {
      "dots": true,
      "dotsClass": "slick-dots custom-dot-class",
      "edgeFriction": 0.35,
      "infinite": false,
      "speed": 500,
      "slidesToShow": 3,
      "slidesToScroll": 1
    },
    center: true,
    blog: null,
    data: null,
    error: null,
    loading: true,
  }),
  methods:{
    HandleClick(){
      
    }
  },
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
</script>

<style>
  .titulo {
    font-size: 40px;
    color: pink !important; 
    text-decoration: none;
  }
</style>