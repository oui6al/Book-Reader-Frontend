<template>
  <div>
    <bookReader :bookId="bookId"></BookReader>
  </div>
</template>

<script>
import bookReader from "../components/bookReader.vue";

  export default {
    components: {
      bookReader
    },
    data() {
      return {
        bookId: this.$route.params.id
      };
    },
    
  };
  </script>

