<template>
  <div class="about" v-if="data">
      <p>{{data.blogpost.titulo}}</p>
      <p>{{data.blogpost.descripcion}}</p>
       <datocms-image :data="data.blogpost.photo.responsiveImage" />
    </div>
</template>
  

<script>
import { request } from "@/datocms";
import { Image } from "vue-datocms";

const POST_QUERY = `query MyQuery {
  blogpost {
    titulo
    descripcion
    photo {
      responsiveImage {
        src
        srcSet
        base64
        aspectRatio
        width
        sizes
      }
    }
  }
}`;

export default {
  name: 'Home',
   components: {
    "datocms-image": Image
  },
  data() {
    return {
      data: null,
      error: null,
      loading: true,
    }
  },
  async created() {
    try {
      this.data = await request({
        query: POST_QUERY,
        variables: {
          limit: 10
        }
      });
    } catch (e) {
      this.error = e;
    }
    this.loading = false;
  }
}
</script>
