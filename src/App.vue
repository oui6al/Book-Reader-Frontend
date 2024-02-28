
<script>
import { RouterLink, RouterView } from 'vue-router'
export default {
  mounted(){
    sessionStorage.removeItem('searchQuery');
  }
}
</script>

<template>
  <RouterView />
</template>

