<style>

.header-search {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 18px;
  padding: 10px;
}

#search-input {
  width: calc(50% + 10px); 
  max-width: 300px; 
}

.grid-container {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 20px;
  max-width: 1400px;
  margin: 0 auto; 
  padding: 20px 10px; 
}

@media screen and (max-width: 768px) {
  .grid-container {
    grid-template-columns: repeat(auto-fill, minmax(150px, 1fr)); 
    padding: 20px 5px; 
  }
}

.grid-container img {
  max-width: 100%;
  height: auto;
}

.image-name {
  text-align: center;
  margin-top: 10px;
  font-weight: bold;
}


</style>

<template>
  <div class="container">
    <div class="header-search">
      <h1>Pesquise seu Gif preferido!</h1>
      <input type="text" v-model="searchTerm" id="search-input" placeholder="Search GIFs">
    </div>

    <div v-if="gifsData.length > 0" class="grid-container">
      <div class="image-container" v-for="gif in filteredGifs" :key="gif.id">
        <img :src="gif.images.fixed_height.url" :alt="gif.title">
        <div class="image-name">{{ gif.title || gif.username || 'Untitled' }}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      apiKey: 'gFawHwGFmydNnaWqSZpAPEuCqQkNqf22',
      apiUrl: 'https://api.giphy.com/v1/gifs/trending',
      searchTerm: '',
      gifsData: [],
      filteredGifs: [] // Adicionando uma propriedade para armazenar os gifs filtrados
    };
  },
  methods: {
    filterGifs() {
      this.filteredGifs = this.gifsData.filter(gif => {
        return gif.title.toLowerCase().includes(this.searchTerm.toLowerCase());
      });
    }
  },
  mounted() {
    const apiUrlWithParams = `${this.apiUrl}?api_key=${this.apiKey}&limit=25&offset=0&rating=g&bundle=messaging_non_clips`;
    fetch(apiUrlWithParams)
      .then(response => {
        if (!response.ok) {
          throw new Error('Network response was not ok');
        }
        return response.json();
      })
      .then(data => {
        this.gifsData = data.data; // Armazenar dados das imagens para facilitar a filtragem
        this.filterGifs(); // Chama a função para filtrar os gifs assim que os dados são carregados
      })
      .catch(error => {
        console.error('There was a problem with the fetch operation:', error);
      });
  },
  watch: {
    searchTerm: function(newVal) {
      this.filterGifs();
    }
  }
};
</script>

