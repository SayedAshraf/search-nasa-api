<template>
  <div class="container search">
    <div class="search-header">
      <h1>NASA Search</h1>
      <b-form @submit.prevent="getResult(query)">
        <b-input type="text" placeholder="type in your search" v-model="query" />
      </b-form>
    </div>
    <b-alert v-if="!results[0] && !DisplayResults[0] " show variant="danger">Search like sun , earth , moon .... No Results! 😢</b-alert>
    <div class="container p-4  text-center" align-v="center">
      <div v-for="(result, indx) in DisplayResults" :key="indx" class="row">
        <b-img :src="result.links[0].href" :alt="result.data[0].description" ></b-img>
        <!-- <h3>{{ result.data[0].description }}</h3> -->
      </div>
    </div>
    <div class="text-center" v-if="results && results.length != 0">
      <button type="button" class="btn" @click="loadmore">
        Load more
      </button>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Search",
  data() {
    return {
      query: "",
      results: [],
      DisplayResults: [],
    };
  },

  methods: {
    getResult(query) {
      axios
        .get(
          "https://images-api.nasa.gov/search?q=" + query + "&media_type=image"
        )
        .then((res) => {
          this.results = res.data.collection.items;
          this.DisplayResults = this.results.splice(0, 10);
          if (query == "") {
            this.results = [];
            this.DisplayResults = [];
          }
        })
        .catch((err) => console.log(err));
    },
    loadmore() {
      const obj1 = this.results.splice(0, 10);
      this.DisplayResults = [...this.DisplayResults, ...obj1];
      console.log("final");
      console.log(this.DisplayResults);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.search-header {
  margin: 30px 0 15px 0;
  padding: 30px;
  background-color: #d1ecf1;
  border-radius: 20px;

  .alert-form {
    h1 {
      padding: 20px;
    }
    .form-control {
      padding: 0 10px;
      border-radius: 10px;
    }
  }
}
img {
  padding: 10px 0;
  width: 100%;
}
.btn {
  background-color: #d1ecf1 !important;
  border: 1px solid #d1ecf1;
  color: #0c424c;
  border-radius: 5px;
  padding: 10px 25px;
  font-size: 18px;
  border: none;
  font-weight: bold;
  line-height: 1;
  margin: 50px 0;
  &:hover,
  &:focus {
    border: none;
  }
}
</style>
