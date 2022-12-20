<script>
    import Nav from './components/nav.svelte';

    let menuItems = [
        {link: '#', name: 'Docs'},
        {link: '#', name: 'Home'}
    ];

    const apiKey = 'VEQB36CS6FYOPPUO';
    //const apiKey = '2PD6SXM6VTYLAZ5D';

    let ticker = 'GOOG';

    async function getStockData() {
        let response = await fetch(
            'https://www.alphavantage.co/query?function=OVERVIEW&symbol=' + ticker + '&apikey=' + apiKey
        );
        let stockData = await response.json();
        return stockData;
    }
    let promise = getStockData();
</script>

<Nav {menuItems} />

<input bind:value={ticker} />
<button on:click={getStockData}>Click to Load Stock Data</button>

<div>
    {#await promise}
        <p>Loading...</p>
    {:then stockData}
        <tbody>
            {#each Object.entries(stockData) as row}
                <tr>
                    {#each row as cell}
                        <td>{cell}</td>
                    {/each}
                </tr>
            {/each}
        </tbody>
    {:catch error}
        <p style="color: red">{error.message}</p>
    {/await}
</div>
