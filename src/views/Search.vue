<template>
  <div class="wrapper">
    <div class="search">
      <label for="search">Search </label>
      <input 
      id="search" 
      type="search" 
      v-model="searchValue" 
      @input="handleInput" 
      />
      <ul>
        <li v-for="item in results" :key="item.data[0].nasa_id"> <!-- pętla wypisójąca nazwy  -->
          <p>{{ item.data[0].description  }}</p>
        </li>
      </ul>
    </div>
  </div>
</template>

<!-- @input="handleInput" - wywołuje metodę handleInput która wypsiuje zawartość inputa za każdym razem kiedy się coś wpiszę -->


<script>
import axios from "axios"; //importuje axios
import debounce from 'lodash.debounce';

const API = "https://images-api.nasa.gov/search";

export default {
  name: 'Search',
  data() {
    return {
      searchValue: '', //zmiena w niej znajduje się value input
      results: [],
    };
  },
  methods: {
    handleInput: debounce(function() {//zapytanie
      axios.get(`${API}?q=${this.searchValue}&media_type=image`) 
        .then((response) => { //poprawne zapytanie
            this.results = response.data.collection.items;
        })
        .catch((error) => { // łapie błędy 
          console.log(error);
      });
    },500),
  },
};
</script>

<style lang="scss" scoped>
  .wrapper {
    width:100%;
    padding:30px;
    margin:0px;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .search {
    width:250px;
    display:flex;
    flex-direction: column;
  }

  label {
    font-family: Montserrot, sans-serif;
  }

  input {
    height: 30px;
    border:0px;
    border-bottom:1px solid black;
  }

</style>
