<script>
  import Card from "./components/Card.svelte";
  import { onMount } from "svelte";

  const API =
    "https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false";
  let coins = [];
  let titles = ["#", "Coin", "Price", "Price Change", "24h Volume"];
  let filterCoins = [];
  let ref = null;

  const loadCoins = async () => {
    const res = await fetch(API);
    const data = await res.json();
    coins = [...data];
    filterCoins = [...data];
  };

  loadCoins();

  const searchCoin = (value) => {
    console.log(value);
    filterCoins = coins.filter(
      (coin) =>
        coin.name.toLowerCase().includes(value.toLowerCase()) ||
        coin.symbol.toLowerCase().includes(value.toLowerCase())
    );
  };

  onMount(() => {
    ref.focus();
  });
</script>

<main class="container">
  <div class="row">
    <h1>Crypto Market</h1>
    <input
      type="text"
      class="svelte-style form-control py-2 bg-dark text-white border-0 rounded-1 my-4"
      placeholder="Search your coin"
      on:keyup={({ target: { value } }) => searchCoin(value)}
      bind:this={ref}
    />
    <table class="table table-dark">
      <thead>
        <tr>
          {#each titles as title}
            <th>{title}</th>
          {/each}
        </tr>
      </thead>
      <tbody>
        {#each filterCoins as coin, i}
          <Card
            image={coin.image}
            name={coin.name}
            price={coin.current_price}
            change={coin.price_change_percentage_24h}
            volume={coin.total_volume}
            symbol={coin.symbol}
            {i}
          />
        {/each}
      </tbody>
    </table>
  </div>
</main>

<style>
  @import url("https://fonts.googleapis.com/css2?family=Share+Tech&family=Share+Tech+Mono&display=swap");
  :global(:root) {
    --dark-color: #212529;
    --main-color: #ffcd46;
  }

  :global(body) {
    background-color: var(--dark-color);
    font-family: "Share Tech Mono", monospace;
  }

  main {
    font-family: "Share Tech", sans-serif;
  }

  h1 {
    font-family: "Share Tech Mono", monospace;
    color: var(--main-color);
    text-align: center;
    font-size: 3rem;
    place-items: center;
  }

  input.svelte-style {
    box-shadow: 0 0 4px gray;
  }
</style>
