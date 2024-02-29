<template>
    <div>
      <button @click="goBack">Retour</button>
      <div v-html="htmlContent"></div>
    </div>
</template>
  
<script>
import axios from 'axios';
import { server } from '../config';
export default {
  props: {
    bookId: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      htmlContent: ""
    };
  },
  mounted() {
      axios.get(server + '/api/book/'+ this.bookId)
      .then((response) => {
         axios.post("http://localhost:3000/api/getContent/",{url: response.data.formats['text/html']}).then((response) => {
            this.htmlContent = response.data;
         });
     });
  },
  methods: {
    goBack() {
      this.$router.back();
    },
  },
}
</script>
  