<template>
  <div class="wrapper">
    <Claim />
    <SearchInput />
  </div>
</template>

<!-- @input="handleInput" - wywołuje metodę handleInput która wypsiuje zawartość inputa za każdym razem kiedy się coś wpiszę -->


<script>
import axios from "axios"; //importuje axios
import debounce from 'lodash.debounce';
import Claim from '@/components/Claim.vue';  
import SearchInput from '@/components/SearchInput.vue';

const API = "https://images-api.nasa.gov/search";

export default {
  name: 'Search',
  components: {
    Claim,
    SearchInput,
  },
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
    height:100vh;
    padding:30px;
    margin:0px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    background: url('../assets/background.jpg');
    background-repeat: no-repeat;
    background-size: cover;
    background-position: 80% 0%;
  }

</style>
