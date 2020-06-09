<template>
  <div class="hello">
    <input type="text" v-on:keyup="updateList" v-model="searchTerm">
    <select v-model="value" v-on:click="changeValue">
      <option value="&type=movie">Movies</option>
      <option value="&type=series">Series</option>
      <option value="&type=episode">Episode</option>
    </select>
    <ul v-if="list">
      <li v-for="(item, index) in list" v-bind:key="index">
          <img v-bind:src="item.Poster" alt="" >
        <h2> {{item.Title}} {{item.Year}}  </h2>
        <p> {{item.Type}} </p>
      </li>
    </ul>
    <p v-if="error">{{this.error}}</p>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'HelloWorld',
  data() {
    return {
      apiKey: 'ae7630a3',
      searchTerm: '',
      error: "Search for something",
      list: null,
      value: "&type=movie"
    }
  },
  methods: {
    updateList() {
      axios
        .get('http://www.omdbapi.com/?s=' + this.searchTerm + this.value + ' &apikey=ae7630a3')
        .then(response => {
          if(response.data.Response == "True") {
          this.list = response.data.Search;
          this.error = null;
          } else {
            if(response.data.Error == "Something went wrong.")
            {
              this.error = "Search for something."
            } else {
              this.error = response.data.Error;
            }
            this.list = null;
          }
          console.log(response);
        })
        .catch(this.error = "Something went wrong, please try again later")
    },
    
    changeValue() {
      if(this.value == "&type=movie") {
        this.searchTerm = "";
      }
      if(this.value == "&type=series") {
        this.searchTerm = "";
      }
      if(this.value == "&type=episode") {
        this.searchTerm = "";
      }
    }
    
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

</style>
