
<template>
  <div class="book-gallery">
    <div class="gallery-container">
      <div class="gallery" ref="gallery">
        <div v-for="book in books" :key="book.id" class="book-item">
          <img :src="book.formats['image/jpeg']" alt="Book Cover" class="book-image" @click="readBook(book.id)"/>
        </div>
      </div>
    </div>
    <!--<button class="navigation-button" @click="scrollRight" >
      <font-awesome-icon icon="chevron-right" size="xxl" />
    </button>-->
  </div>
</template>

<script>
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
import { library } from '@fortawesome/fontawesome-svg-core';
import { faChevronRight } from '@fortawesome/free-solid-svg-icons';

library.add(faChevronRight);

export default {
  components: {
    FontAwesomeIcon,
  },
  props: {
    books: {
      type: Array,
      required: true,
    },
  },
  data(){
    return {
      scrollPosition: 0,
    }
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

.navigation-button {
  position: absolute;
  top: 50%;
  right: 10px;
  transform: translateY(-50%);
  border: none;
  outline: none;
  cursor: pointer;
  width: 60px;
  height: 60px;
}

</style>