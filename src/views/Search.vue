<template>
  <div class="wrapper">
    <div class="search">
      <label for="search">Search</label>
      <input name="search" id="search" v-model="searchValue" @input="handleInput">
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import debounce from 'lodash.debounce';

const API = 'https://images-api.nasa.gov/search';

export default {
  name: 'Search',
  data() {
    return {
      searchValue: '',
      results: [],
    };
  },
  methods: {
    handleInput: debounce(function() {
      axios.get(`${API}?q=${this.searchValue}&media_type=image`)
        .then((response) => {
          this.results = response.data.collection.items;
        })
        .catch((error) => {
          console.log(error);
        });
    }, 500),
  },
};

</script>
<style lang="scss" scoped>
  .wrapper {
    margin: 0;
    padding: 30px;
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .search {
    width: 250px;
    display: flex;
    flex-direction: column;
    align-items: center;

    input {
      height: 50px;
      border: 0;
      border-bottom: 1px solid black;
    }

    label {
      font-family: Montserrat, sans-serif;
    }
  }

</style>
