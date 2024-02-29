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
      result_title: '',
      isAdvancedSearch: false,
      query: '',
    }
  },
  methods: {
    advancedSearch(searchQuery){
      sessionStorage.setItem('searchQuery', searchQuery);
      sessionStorage.setItem('advancedSearch', true);
      this.query = searchQuery;
      this.isAdvancedSearch = true;
      axios.post('http://localhost:3000/api/advanced-search/', {query: searchQuery})
      .then((response) => {
        console.log(response.data);
        this.books = response.data;
        this.result_title = "Résultats de la recherche :"
      })
    },
    simpleSearch(searchQuery){  
      this.query = searchQuery;
      sessionStorage.setItem('searchQuery', searchQuery);
      sessionStorage.setItem('advancedSearch', false);
      this.isAdvancedSearch = false;
      axios.post('http://localhost:3000/api/search', {query: searchQuery})
      .then((response) => {
        console.log(response.data);
        this.books = response.data;
        this.result_title = "Résultats de la recherche :"
      })
    },
    resort(sortOption){
      console.log(sortOption);
      axios.post('http://localhost:3000/api/sort', {books: this.books.map(book => book.id), option: sortOption})
      .then((response) => {
        console.log(response.data);
        this.books = response.data;
        this.result_title = "Résultats de la recherche triés par : " + sortOption;
      })
    }
  },
  mounted(){
    const searchQuery = sessionStorage.getItem('searchQuery');
    if(searchQuery && sessionStorage.getItem('advancedSearch') === 'true'){
      this.advancedSearch(sessionStorage.getItem('searchQuery'));
    }
    else if(searchQuery && sessionStorage.getItem('advancedSearch') === 'false'){
      this.simpleSearch(sessionStorage.getItem('searchQuery'));
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
            <img  src="https://mir-s3-cdn-cf.behance.net/project_modules/max_1200/15ba25105957829.5f851d78e1c3a.jpg" />
          </div>
          <div  id="mid">
            <img id="mid" src="https://mpd-biblio-covers.imgix.net/9781466804807.jpg" />
          </div>
          <div class="figures">
            <img src="https://lauradesignsite.files.wordpress.com/2016/02/13.jpg"/>
          </div>
      </div>
    </div>
    <Search @advanced-search="(query) => advancedSearch(query)" 
      @simple-search="(query) => simpleSearch(query)"
      />
  </div>
  <div>
    <select id="sortOption">
      <option value="relevancy" @click="(isAdvancedSearch) ? advancedSearch(query) : simpleSearch(query)">Relevancy</option>
      <option value="betweeness" @click="resort('betweenness')">Betweeness</option>
      <option value="closeness" @click="resort('closeness')">Closeness</option>
    </select>
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
  height: 35%;
}

.brand {
  display: flex;
  flex-direction: row;
  height: 30rem;
  padding: 3rem;

}

.presentation {
  display: flex;
  flex-direction: column;
  width: 100rem;
 
}
.title {
  font-family: Georgia, 'Times New Roman', Times, serif;
  color: #2c3e50;
  text-justify:inter-word;
  height: 50%;
  font-size: 2rem;
}

.logo {
  width: 70%;
  height: 100%;
  object-fit:fill;
}


.figures-wrapper {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  width: 100%;
  height: 25rem;
  
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
  object-fit:fill;
  border-radius: 0 0rem 10rem 10rem;
}

.figures-wrapper img{
  width: 100%;   
  height: 100%;
  object-fit: cover;
  border-radius: 10rem 10rem 0 0;
}

@media (max-width: 800px) {
  .figures-wrapper {
    display: none;
  }
  .presentation {
    align-items: center;
  }
  .title {
    text-align: justify;
  }
}


</style>
