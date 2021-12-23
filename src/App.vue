<template>
  <div class="container">
    <div class="row">
      <input
        type="text"
        class="form-control bg-dark text-light rounded-0 border-0 my-4"
        placeholder="Search Coin"
        @keyup="searchCoin()"
        v-model="textSearch"
      />
      <table className="table table-dark table-hover mt-4">
        <thead>
          <tr>
            <th v-for="title in titles" :key="title">{{ title }}</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(coin, index) in filteredCoins" :key="coin.id">
            <td class="text-muted">{{ index + 1 }}</td>
            <td>
              <img
                :src="coin.image"
                :alt="coin.name"
                width="50"
                height="50"
                class="me-4 img-fluid"
              />
              <span>{{ coin.name }}</span>
              <span class="ms-3 text-muted text-uppercase">{{
                coin.symbol
              }}</span>
            </td>
            <td>{{ coin.current_price }}</td>
            <td
              :class="[
                coin.price_change_percentage_24h > 0
                  ? 'text-success'
                  : 'text-danger',
              ]"
            >
              {{ coin.price_change_percentage_24h }} %
            </td>
            <td>$ {{ coin.total_volume.toLocaleString() }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "App",
  data() {
    return {
      coins: [],
      filteredCoins: [],
      titles: ["#", "Coin", "Price", "Change", "24h Volumen"],
      textSearch: "",
    };
  },
  async mounted() {
    const res = await axios.get(
      "https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false"
    );
    this.coins = res.data;
    this.filteredCoins = res.data;
  },
  methods: {
    searchCoin() {
      this.filteredCoins = this.coins.filter((coin) => {
        return (
          coin.name.toLowerCase().includes(this.textSearch.toLowerCase()) ||
          coin.symbol.toLowerCase().includes(this.textSearch.toLowerCase())
        );
      });
    },
  },
};
</script>

<style>
#app {
}
</style>
