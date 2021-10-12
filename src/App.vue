<template>
  <div :class="darkmode ? 'uk-background-secondary': ''" class="home uk-padding">
    <div class="uk-flex uk-flex-between uk-flex-middle">
      <h1 :class="darkmode ? 'text-white':''" class="uk-text-lighter">Coin Market</h1>
      <div>
        <button @click="darkmode = !darkmode" uk-icon="cog"></button>
      </div>
    </div>
    <form class="uk-width-expand uk-search uk-search-default uk-margin-small-top uk-margin-small-bottom">
      <input class="uk-search-input" type="search" placeholder="Search" @keyup="handleSearch">
    </form>
    <table class="uk-table uk-table-divider uk-table-responsive uk-table-hover">
      <thead>
          <tr>
              <th>Name</th>
              <th>Symbol</th>
              <th>Image</th>
              <th>Price</th>
              <th>Price Change(%)</th>
              <th>Volumen 24h</th>
          </tr>
      </thead>
      <tbody v-if="filterSearch.length>0">
          <CoinItem v-for="coin in filterSearch" :key="coin.id" :coin="coin"/>
      </tbody>
      <p v-else>no hay resultados</p>
    </table>
    <div class="list-pages uk-flex uk-flex-center">
      <button @click="handleChangePages" v-for="page in listpage" :key="page" class="uk-button-primary" style="padding: 5px 10px;border-radius: 3px">{{page}}</button>
    </div>
  </div>
</template>

<script>
import CoinItem from "./components/CoinItem.vue"


export default {
  name: 'App',
  components: { 
    CoinItem
  },
  data() {
    return {
      coin_data: [],
      filterSearch: [],
      listpage: [1,2,3,4,5,6,7],
      darkmode: false
    }
  },
  methods: {
    handleSearch(e){
      e.preventDefault();
      
      this.filterSearch = this.coin_data.filter(coin => 
        coin.name.toLowerCase().includes(e.target.value.toLowerCase()) ||
        coin.symbol.toLowerCase().includes(e.target.value.toLowerCase())
      );
    },
    handleChangePages(e){
      this.fetchCoins(parseInt(e.target.textContent))
    },
    async fetchCoins(value){
      const res = await fetch(`https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=8&page=${value}&sparkline=false`) 
      const data = await res.json();
      this.coin_data = data;
      this.filterSearch = data;
    },
  },
  async mounted() {
    this.fetchCoins(1)
  },
}
</script>

<style>
html,body{
  height: 100%;
  padding: 0;
  margin: 0;
}

.home{
  min-height: 100vh;
}

.text-white{
  color: white;
}

.list-pages{
  grid-gap: 6px;
}

.list-pages button{
  cursor: pointer;
}
</style>
