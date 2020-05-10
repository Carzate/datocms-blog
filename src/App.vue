<template>
  <v-app>
      <Header class="mb-12"/>
      <router-view class="mt-12"/>
  </v-app>
</template>

<script>
import Header from '@/components/Header.vue'
export default {
  components: {
    Header
  }
}
</script>