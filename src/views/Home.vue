<template>
  <moviesListing></moviesListing>
</template>

<script>
// @ is an alias to /src
import moviesListing from '@/components/moviesListing.vue'

export default {
  name: 'Home',
  components: {
    moviesListing
  }
}
</script>
