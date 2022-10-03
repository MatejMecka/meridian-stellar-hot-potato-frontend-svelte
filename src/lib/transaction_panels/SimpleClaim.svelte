<script lang="ts">
    import Button, { Label, Icon } from '@smui/button';
    import CircularProgress from '@smui/circular-progress';

    const BACKEND_URL = import.meta.env.VITE_BACKEND_URL
    const SIMPLE_SIGNER_URL = import.meta.env.VITE_SIMPLE_SIGNER_URL
    export let xdr
    export let type
    let response_data = undefined

    // Yes yes, I know it's a duplicate...
    const transaction_signature = (async () => {
        window.open(
            `${SIMPLE_SIGNER_URL}/sign?xdr=${xdr}`,
            'Connect_Window',
            'width=360, height=450',
        );
    })

    async function fetchXDR(){
        if(type != "signed"){
            return
        }
        const response = await fetch(`${BACKEND_URL}/submit-transaction`, {
            method: 'POST',
            body: JSON.stringify({
                xdr: xdr,
            }),
            headers: {
                "Content-Type": "application/json"
            }
        })
        const data = await response.json()
        response_data = data['xdr']
        return data
    }

    let submit_xdr = fetchXDR()

    function handle_promise (){
        console.log('EXECUTED')
        if(type == "signed"){
         submit_xdr = fetchXDR();
        }
    }

    console.log(type)
</script>
<br>
<b>Sign Transaction with Simple Signer</b>
<p>In order to get your signature, you'll be prompted to select your wallet and authorize the transaction. Once that is done the transaction will be submitted to the network.</p>

<Button
  on:click={transaction_signature}
  variant="unelevated"
  class="button-shaped-round"
>
  <Icon class="material-icons">password</Icon>
  <Label>Sign Transaction</Label>
</Button>

{#if type == "signed"}
<br><br>
    {void handle_promise() ?? ""}
    {#await submit_xdr}
    <div style="display: flex; justify-content: center">
        <CircularProgress style="height: 32px; width: 32px;" indeterminate />
    </div>
    {:then data}
        {#if response_data != undefined}
            <b style="color:green">Transaction signed succesfully!</b>
        {:else if data['detail'] != undefined}
            <b style="color:red">{data['detail']}</b>
        {/if}

    {:catch error}
        <h4>An error occurued: {error}</h4>
    {/await}


{/if}