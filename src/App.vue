<template>
  <nav-bar/>
<router-view/>
</template>

<script>
import NavBar from '@/components/NavBar.vue'
export default {
  components: { NavBar
  },
  name: 'App'
}

</script>


<style>
</style>
