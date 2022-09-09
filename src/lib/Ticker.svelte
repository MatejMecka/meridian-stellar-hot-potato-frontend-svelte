<script lang="ts">
    import CircularProgress from '@smui/circular-progress';
    import CountingDown from '../utils/counting_down.svelte';
    import Card, { Content } from '@smui/card';

    const ISSUER = import.meta.env.VITE_ISSUER
    const HORIZON_URL = import.meta.env.VITE_HORIZON_URL
    const ASSET_CODE = import.meta.env.VITE_ASSET_CODE

    let holder = undefined
    let loading = true
    let msToTime
    let hours, minutes, seconds

    const getTime = (async () => {
        // Fetch Data Fields from Issuer
        const response = await fetch(`${HORIZON_URL}/accounts/${ISSUER}`)
        const data = await response.json()
        holder = atob(data['data']['currentAccount'])

        // Get Metadata
        const trades = await fetch(`${HORIZON_URL}/accounts/${holder}/trades?limit=200&order=desc`)
        const trades_data = await trades.json()

        trades_data["_embedded"]["records"].forEach((trade) => {
              if (
                trade["counter_asset_code"] == ASSET_CODE &&
                trade["counter_asset_issuer"] == ISSUER
              ) {
                console.log(trade);

                const ledger_date = new Date(trade["ledger_close_time"]);
                loading = false
                setInterval(() => {
                  const time_diff = new Date().getTime() - ledger_date.getTime();
                  const remaining_time = 8.64e7 - time_diff; // 24 hours in milliseconds
                  [hours, minutes, seconds] = msToTime(remaining_time);
                
                  hours = hours < 10 ? "0" + hours : hours;
                  minutes = minutes < 10 ? "0" + minutes : minutes;
                  seconds = seconds < 10 ? "0" + seconds : seconds;
                }, 1000);
              }
            });

    })()

</script>

<CountingDown bind:miliToTime={msToTime}></CountingDown>
{#if holder == undefined && loading != true}
    <h1>Stellar Hot Potato</h1>
    <h1><b>Game has not started</b></h1>
{:else if holder == undefined && loading == true}
    <div style="display: flex; justify-content: center">
        <CircularProgress style="height: 32px; width: 32px;" indeterminate />
    </div>
{:else}
    {#if hours > 0}
        <h1>Stellar Hot Potato</h1>
        <h4>Time remaining until {holder} burns the Potato</h4>
        <h1 class="time">{hours}:{minutes}:{seconds}</h1>
    {:else }
    <div class="card-container">
        <Card class="over_card">
            <Content>
                <h1>Stellar Hot Potato is over!</h1>
                <h3>Thank you all for playing and I hope you enjoyed this year's experience :)</h3>
                <h1>😁👋</h1>
                <h6>Feel free to share your experience on Twitter or on Stellar Global's Discord :D Hope to see you in the future :D</h6>
            </Content>
        </Card>
    </div>
     {/if}
{/if}
<h1>🥔</h1>

<style>
    .time {
        background-color: black;
        border-radius: 25px;
        color: white;
    }

    @media (prefers-color-scheme: dark) {
        :global(.over_card){
            background-color: var(--mdc-theme-surface, #212125) !important;
        }
    }

    
</style>