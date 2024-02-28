
<template>
  <div class="book-gallery">
    <h2 :title="title">{{ title }} </h2> 
    <div class="gallery-container">
      <div class="gallery" ref="gallery">
        <div v-for="book in books" :key="book.id" class="book-item">
          <img :src="book.formats['image/jpeg']" alt="Book Cover" class="book-image" @click="readBook(book.id)"/>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  components: {
   
  },
  props: {
    title: {
      type: String,
      required: true,
    },
    books: {
      type: Array,
      required: true,
    },
  },
  methods: {
    readBook(bookId) {
      this.$router.push({ name: 'reader', params: { id: bookId } });
      console.log("readBook", bookId);
    },
}
    
};
</script>

<style scoped>

.book-gallery {
  position: relative;
  width: 100%;
  overflow: hidden;
  margin-top: 3rem;
}

.gallery-container {
  width: 100%;
  overflow-x:hidden;
  position: relative;
  justify-content: center;
  margin-top: 3rem;
}


.gallery {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: center;
}

.book-item {
  flex: 0 0 auto;
  margin: 10px;
  padding: 1rem;
  background-color: #f7f6f0;
}

.book-image {
  width: 150px;
  height: 200px;
  object-fit: cover;
  box-shadow: 10px 10px 10px 10px rgba(0, 0, 0, 0.2);
  border-radius: 10px 5px 0 0;
  cursor: pointer;
}


</style>