<template>
  <div>
    <!-- Galerie principale-->
    <div class="book-gallery">
      <h2 :title="title">{{ title }} </h2>
      <div class="gallery-container">
        <div class="gallery" ref="gallery">
          <div v-for="book in books" :key="book.id" class="book-item">
            <img :src="book.formats['image/jpeg']" alt="Book Cover" class="book-image" @click="readBook(book)" />
          </div>
        </div>
      </div>
    </div>

    <!-- Galerie d'historique -->
    <div class="book-gallery">
      <h2>Historique</h2>
      <div class="gallery-container">
        <div class="gallery" ref="gallery">
          <div v-for="historyBook in historyBooks" :key="historyBook.id" class="book-item">
            <img :src="historyBook.formats['image/jpeg']" alt="Book Cover" class="book-image"
              @click="readBook(historyBook)" />
          </div>
        </div>
      </div>
    </div>

     <!-- Galerie de suggestion -->
     <div class="book-gallery">
      <h2>Suggestion</h2>
      <div class="gallery-container">
        <div class="gallery" ref="gallery">
          <div v-for="suggestionBook in suggestionBooks" :key="suggestionBook.id" class="book-item">
            <img :src="suggestionBook.formats['image/jpeg']" alt="Book Cover" class="book-image"
              @click="readBook(suggestionBook)" />
          </div>
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
    historyBooks: {
      type: Array,
      required: true
    },
    suggestionBooks: {
      type: Array,
      required: true
    }
  },
  methods: {
    readBook(book) {
      this.$router.push({ name: 'reader', params: { id: book.id } });
      console.log("readBook", book.id);

      // Store history
      let storedBooks = JSON.parse(localStorage.getItem('historyBooks')) || [];
      const bookId = book.id;

      const index = storedBooks.findIndex((storedBook) => storedBook.id === bookId);
      if (index !== -1) {
        storedBooks.splice(index, 1);
      }

      storedBooks.unshift(book);
      console.log("History", storedBooks);

      localStorage.setItem('historyBooks', JSON.stringify(storedBooks));
    },
  }
};
</script>

<style scoped>
.container {
  display: flex;
}

.book-gallery {
  position: relative;
  width: 100%;
  overflow: hidden;
  margin-top: 3rem;
}

.gallery-container {
  width: 100%;
  overflow-x: hidden;
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
  object-fit: fill;
  box-shadow: 10px 10px 10px 10px rgba(0, 0, 0, 0.2);
  cursor: pointer;
}
</style>