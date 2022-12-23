<script>
  import Nav from "./components/nav.svelte";

  let menuItems = [
    { link: "#", name: "Docs" },
    { link: "#", name: "Home" },
  ];

  //const apiKey = "VEQB36CS6FYOPPUO";
  const apiKey = "2PD6SXM6VTYLAZ5D";
  let ticker = "";

  async function getStockData() {
    let response = await fetch(
      "https://www.alphavantage.co/query?function=OVERVIEW&symbol=" +
        ticker +
        "&apikey=" +
        apiKey
    );
    let stockData = await response.json();
    return stockData;
  }

  let promise = getStockData;

  function handleClick() {
    promise = getStockData();
  }
</script>

<Nav {menuItems} />
<div class="flex justify-center">
  <input
    bind:value={ticker}
    placeholder="Stock Ticker"
    class="border-2 p-4 m-4"
  />
  <button on:click={handleClick} class="border-2 p-2 m-4">Add Stock</button>
</div>

{#await promise}
  <p>Loading...</p>
{:then stockData}
  {#each Object.keys(stockData) as key}
    <button on:click={() => alert(key)} class="border-2 p-2 m-2">{key}</button>
  {/each}
{:catch error}
  <p style="color: red">{error.message}</p>
{/await}

<div class="w-1/3 m-auto">
  <table class="table p-4 bg-white rounded-lg shadow">
    {#await promise}
      <p>Loading...</p>
    {:then stockData}
      <tbody>
        {#each Object.entries(stockData) as row}
          <tr class="text-gray-700 odd:bg-white even:bg-slate-50">
            {#each row as cell}
              <td class="border-b-2 p-4 dark:border-dark-5 first:font-bold"
                >{cell}</td
              >
            {/each}
          </tr>
        {/each}
      </tbody>
    {:catch error}
      <p style="color: red">{error.message}</p>
    {/await}
  </table>
</div>
