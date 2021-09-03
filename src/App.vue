<template>
  <div id="app">
    <div class="header">
      <img src="./assets/logo.png" style="width:5rem" alt="">
      <h1>VueCryptoMarket</h1>
    </div>
    <div class="buscador">
      <input v-model="textSearch" id="buscador" type="text" class="form-control bg-dark text-light my-4" placeholder="Busca tu crypto preferida" @keyup="searchCoin" >
    </div>
      
        
    <div class="container">
      <div class="row">      
      <table class="table table-dark">
      <thead>
        <tr>
          <th v-for="title in titles" :key="title">
            {{title}}
          </th>
        </tr>
        
      </thead>
      <tbody>
        <tr v-for="(coin, index) in filteredCoins" v-bind:key="coin.id">
          <td class="text-muted">
            {{index +1}}
          </td>
          <td>
              <img  :src=coin.image alt="" style="width:2rem" class="me-2">
              <span>
                {{coin.name}}
              </span>
              <span class="text-uppercase text-muted">
                {{coin.symbol}}
              </span>
          </td>
          <td>
              ${{coin.current_price}}  
          </td>
          <td :class="[coin.price_change_percentage_24h > 0 ? 'text-success' : 'text-danger' ]">
            {{coin.price_change_percentage_24h}}%
          </td>
          <td>
              ${{coin.total_volume.toLocaleString()}}  
          </td>            
        </tr>
        
        
      </tbody>
    </table>
    </div>
    <div class="footer">
      Desarrollado por&nbsp;<a href="https://www.linkedin.com/in/jmalefranch">@jmalefranch</a>
    </div>    
    </div>    
  </div>
</template>

<script>


export default {
  name: 'App',
  data() {
    return {
      coins: [],
      filteredCoins:[],
      titles: [
        '',
        'Moneda',
        'Precio',
        'Cambios',
        'Vol. 24hs',
      ],
      textSearch: '',
    }
  },
  async mounted() {
   const res = await fetch('https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false');
   const data = await res.json();
   this.coins = data;
   this.filteredCoins = data;
   console.log(data)
  },
  methods:{
    searchCoin: function (){
     this.filteredCoins = this.coins.filter(coin => 
     coin.name.toLowerCase().includes(this.textSearch.toLowerCase()) || 
     coin.symbol.toLowerCase().includes(this.textSearch.toLowerCase()))     
    }
  }
  
}
</script>

<style>

</style>
