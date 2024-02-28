<script>
import Search from '../components/Search.vue';
import bookshelves from '../components/bookshelves.vue';
import axios from 'axios';

export default {
  components: {
    Search,
    bookshelves,
  },
  data() {
    return {
      books: [],
      result_title: "Résultats de recherche :"
    }
  },
  methods: {
    advancedSearch(searchQuery){
      axios.post('http://localhost:3000/api/advanced-search/', {query: searchQuery})
      .then((response) => {
        console.log(response.data);
        this.books = response.data;
      })
    },
    simpleSearch(searchQuery){  
      axios.post('http://localhost:3000/api/search', {query: searchQuery})
      .then((response) => {
        console.log(response.data);
        this.books = response.data;
      })
    }
  }
}

</script>


<template>
    <div class="header">
    <div class="brand">
      <div class="presentation">
        <img alt="Vue logo" class="logo" src="../assets/logo.png" />
        <p class="title">Explorez, trouvez, lisez. Cherchez par phrase, mot ou regex. La lecture qui vous correspond,  à portée de clic !</p>
      </div>
      <div class="figures-wrapper">
          <div class="figures">
            <img alt="Brand logo"  src="https://mir-s3-cdn-cf.behance.net/project_modules/max_1200/1bcea8105505099.5f7afff337b57.jpg" />
          </div>
          <div  id="mid">
            <img id="mid" alt="Brand logo"  src="https://mir-s3-cdn-cf.behance.net/project_modules/max_1200/1bcea8105505099.5f7afff337b57.jpg" />
          </div>
          <div class="figures">
            <img alt="Brand logo"  src="https://mir-s3-cdn-cf.behance.net/project_modules/max_1200/1bcea8105505099.5f7afff337b57.jpg"/>
          </div>
      </div>
    </div>
    <Search @advanced-search="(query) => advancedSearch(query)" 
      @simple-search="(query) => simpleSearch(query)"
      />
  </div>
  <bookshelves :books="books" :title="result_title"></bookshelves>
</template>

<style scoped> 
.header {
  display: flex;
  flex-direction: column;
  padding: 1rem;
  background-color:#f7f6f0;
  box-shadow: 0 0 5px 0 rgba(0, 0, 0, 0.2);
  height: 40%;
}

.brand {
  display: flex;
  flex-direction: row;
}

.presentation {
  display: flex;
  flex-direction: column;
  width: 100rem;
}
.title {
  font-family: Georgia, 'Times New Roman', Times, serif;
  color: #2c3e50;
  text-justify:inter-word
}

.logo {
  margin: 0 auto;
  width: 100%;
  height: 50%;
  object-fit:contain;
}


.figures-wrapper {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  width: 100%;
  height: 18rem;
}

.figures {
  width: 30%;
  height: 100%;
  margin: 0.5rem;
  background-color: #f7f6f0;
  border-radius: 10rem 10rem 0 0;
}

.figures-wrapper #mid {
  width: 30%;
  height: 100%;
  border-radius: 0 10rem 10rem 10rem;
  margin-top: 1.1rem;
}

.figures-wrapper #mid img{
  width: 100%;   
  height: 100%;
  object-fit: cover;
  border-radius: 0 0rem 10rem 10rem;
}

.figures-wrapper img{
  width: 100%;   
  height: 100%;
  object-fit: cover;
  border-radius: 10rem 10rem 0 0;
}


</style>
