<template>
  <div>
    <button v-if="!loading" @click="getQuotes()" type="button" class="btn btn-danger">
      New Quotes
    </button>
    <hr>
    <div v-if="loading" class="progress progress-striped active">
      <div class="progress-bar" style="width: 100%"></div>
    </div>
    <ul class="list-group">
      <li class="list-group-item" v-for="quote in quotes">
        <h3>{{quote}}</h3>
        <button @click="viewQuote(quote)" type="button" class="btn btn-primary">
          View Quote
        </button>
      </li>
    </ul>
  </div>
</template>

<script>
import { setQuote } from './QuoteManager';

const RON_SWANSON_API = 'https://ron-swanson-quotes.herokuapp.com/v2/quotes/10';

export default {
  name: 'search',
  data() {
    return {
      quotes: [],
      loading: false,
    };
  },
  mounted() {
    this.getQuotes();
  },
  methods: {
    getQuotes() {
      this.loading = true;
      fetch(RON_SWANSON_API)
        .then(res => res.json())
        .then((quotes) => {
          this.quotes = quotes;
          this.loading = false;
        });
    },
    viewQuote(quote) {
      setQuote(quote);
      this.$router.push('/view');
    },
  },
};
</script>
