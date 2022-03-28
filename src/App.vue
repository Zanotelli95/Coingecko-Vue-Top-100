<template>
<div class="container">
<div class="row"> 

    <h1> Top 100 Coin Market  </h1> 

    <input type="text" class="form-control bg-dark text-light rounder-0 border-0 my-4"
    placeholder="Buscar cripto:"
    @keyup="searchCoin()"
    v-model="textSearch"
    >

  <table class="table table-dark">
   <thead> 
      <tr>
      <th v-for="title in titles" :key="title" >
          {{title}}
        </th>
      </tr>
   </thead> 
   <tbody>
    <tr v-for="(coin, index) in filteredCoins" :key="coin.id">
      <td class="text-muted">
        {{index + 1}}
      </td>
      <td>
        <img :src="coin.image" style="width:1rem"  class="me-2">
        <span>
           {{coin.name}}
        </span>
        <span class="ms-2 text-uppercase text-muted">
            {{coin.symbol}}
        </span>
      </td>
      <td>
        ${{coin.current_price}}
      </td>
      <td :class="[coin.price_change_percentage_24h > 0 ? 'text-success' : 'text-danger']">
        {{coin.price_change_percentage_24h}}%
      </td>
      <td>
        ${{coin.total_volume.toLocaleString()}}
      </td>
      <td>
        ${{coin.circulating_supply.toLocaleString()}}
      </td>
      <td>
        ${{coin.ath}}
      </td>
    </tr>     
  </tbody>
  </table> 
  <footer>
    <h6>Por: Fabián Zanotelli Oviedo</h6>
  </footer>
</div>

</div>
</template>

<script>


export default {
  name: 'App',
  data(){
    return {
        coins: [],
         filteredCoins: [],
        titles: ['#',
          'Coin',
          'Price',
          'Price change',
          '24h Volume',
          'Current Circulating Supply',
          'All Time High'
          ],
        textSearch: ''
    };
  },
  async mounted() {
    const res = await fetch('https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false')
    const data = await res.json()
    console.log(data);
    this.coins = data;
    this.filteredCoins = data
    },  
    methods: {

        searchCoin() {
            this.filteredCoins = this.coins.filter((coin) =>
             coin.name.toLowerCase().includes(this.textSearch.toLowerCase()) ||
                coin.symbol.toLowerCase().includes(this.textSearch.toLowerCase())
            );
          },
    },
  
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
