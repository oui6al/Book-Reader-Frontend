<template>
    <div class="search-bar">
        <input type="text" id="input" @keyup.enter="search()" v-model="searchQuery" :placeholder="advancedSearch ? 'Veuillez entrez une expression régulière' : 'Chercher un titre...'" />
        <button @click="toggleSwitchButton" id="switch-button">
            {{ switchButtonText }}
        </button>
    </div>
</template>


<script>

export default {
    data() {
        return {
            searchQuery: "",
            advancedSearch: false,
            switchButtonColor: "#000080",
            switchButtonText: "Recherche simple",
        };
    },
    methods: {
        toggleSwitchButton() {
            if (this.switchButtonText === "Recherche avancée") {
                this.switchButtonText = "Recherche simple"
                this.advancedSearch = false;
            } else {
                this.switchButtonText = "Recherche avancée";
                this.advancedSearch = true;
            }
        },
        search(){
            if(this.advancedSearch){
                this.$emit('advanced-search', this.searchQuery);
            }
            else{
                this.$emit('simple-search', this.searchQuery);  
            }
        }
    },
    mounted(){
        const query = sessionStorage.getItem('searchQuery');
        if(query){
            this.searchQuery = query;
        }
    }
};
</script>

<style scoped>
.search-bar {
    display: flex;
    flex-direction:row;
    justify-content: space-between; 
    padding: 1rem;
    border-radius: 5px;
    box-shadow: 0 0 5px 0 rgba(0, 0, 0, 0.2);
    background-color: white;
    width: 100;
}

.search-bar #input {
    border: none;
    border-radius: 5px;
    font-size: 1rem;
    width: 100%;
    margin-left: 1rem;
    margin-right: 1rem;
    border-style: solid;
}

#switch-button {
    background-color: #303030;
    color: white;
    border: none;
    border-radius: 5px;
    padding: 0.5rem 1rem;
    cursor: pointer;
    font-size: medium;
    font-family: Georgia, 'Times New Roman', Times, serif;
}


</style>
