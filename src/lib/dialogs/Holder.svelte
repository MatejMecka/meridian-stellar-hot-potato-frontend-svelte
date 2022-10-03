<script lang="ts">
    import Dialog, { Title, Content, Actions } from '@smui/dialog';
    import Button, { Label } from '@smui/button';
    import Textfield from '@smui/textfield';
    import Icon from '@smui/textfield/icon';
    import Tab from '@smui/tab';
    import TabBar from '@smui/tab-bar';
    import CircularProgress from '@smui/circular-progress';
    import SimpleTransaction from '../transaction_panels/SimpleTransaction.svelte';
    import AdvancedTransaction from '../transaction_panels/AdvancedTransaction.svelte';

   const BACKEND_URL = import.meta.env.VITE_BACKEND_URL
   const SIMPLE_SIGNER_URL = import.meta.env.VITE_SIMPLE_SIGNER_URL

    export let open = false
    export let holder = ""
    export let signedXDR = ""
    let destination = ""
    let regex = new RegExp('G[A-Z-0-9]{54}');

    let tabs = [
        {
            icon: "star",
            label: "Simple"
        }, 
        {
            icon: "biotech",
            label: "Advanced"
        }
    ]

    let active = undefined
    let xdr = undefined
    let xdr_to_send = undefined

    async function fetchXDR(){
        if(holder == "" || destination == ""){
            return
        }

        const response = await fetch(`${BACKEND_URL}/exchange`, {
            method: 'POST',
            body: JSON.stringify({
                sender: holder,
                receiver: destination
            }),
            headers: {
                "Content-Type": "application/json"
            }
        })
        const data = await response.json()
        xdr = data['xdr']
        pickXDR()
        return data
    }

    let promise_xdr = fetchXDR()

    function handle_promise (){
        if(regex.test(destination)){
            promise_xdr = fetchXDR();
        }
    }

    const transaction_signature = (async () => {
        window.open(
            `${SIMPLE_SIGNER_URL}/sign?xdr=${xdr}`,
            'Connect_Window',
            'width=360, height=450',
        );
    })

    const pickXDR = function () {
        console.log("EXECUTED")
        console.log(xdr)
        console.log(signedXDR)
        if (signedXDR == ""){
            xdr_to_send = xdr
        } else {
            xdr_to_send = signedXDR
        }
        console.log(xdr_to_send)
    }


</script>

<Dialog
  bind:open
  scrimClickAction=""
  escapeKeyAction=""
  aria-labelledby="mandatory-title"
  aria-describedby="mandatory-content"
  class="dialog_card"
  fullscreen
>
  <Title id="mandatory-title">Woohooo! You're in a posession of a 🥔</Title>
  <Content id="mandatory-content">
    <b>Step 1. Obtain a Destination Address</b><br>
    In order to pass the potato, you firstly need someone to accept it. Ask the recepient for their stellar wallet's public key and fill it here:
    <br>
    <Textfield bind:value={destination} label="Destination" on:input={handle_promise} required pattern="G[A-Z-0-9]{54}">
        <Icon class="material-icons" slot="trailingIcon">person</Icon>
      </Textfield>

    {#if regex.test(destination)}
        <br><br>
        {#await promise_xdr}
            <div style="display: flex; justify-content: center">
                <CircularProgress style="height: 32px; width: 32px;" indeterminate />
            </div>
        {:then data}
        
        {#if xdr != undefined}
        <b>Step 2: Provide your own signature (Optional)</b><br>
            A Transaction has been generated for you. You can sign the transaction with Simple Signer and then generate a link with Hot Potato or use the XDR from Advanced Mode and do everything manually.
        <br><br>
        <Button
            on:click={transaction_signature}
            variant="unelevated"
            class="button-shaped-round"
            >
            <Icon class="material-icons">password</Icon>
            <Label>Sign Transaction</Label>
            </Button>

        {#if signedXDR != ""}
        <br><br>
            {void pickXDR() ?? ""}
            <b style="color:green">Transaction signed succesfully!</b>
        {/if}

        {:else if data['detail'] != undefined}
            <b style="color:red">{data['detail']}</b>
        {/if}

        {:catch error}
            <h4>An error occurued: {error}</h4>
        {/await}

        <br><br>
        {#if xdr != undefined}    
            <b>Step 3. Pass the signed transaction to the destination account.</b><br> You have 2 options on how you can achieve this. Select the one you're most comfortable with.
            <TabBar {tabs} let:tab bind:active>
                <Tab {tab}>
                <Icon class="material-icons">{tab.icon}</Icon>
                <Label>{tab.label}</Label>
                </Tab>
            </TabBar>
            
            {#if active == tabs[0]}
                <SimpleTransaction bind:xdr={xdr_to_send}></SimpleTransaction>
            {/if}

            {#if active == tabs[1]}
                <AdvancedTransaction bind:xdr={xdr_to_send}></AdvancedTransaction>
            {/if}

        {/if}

    {/if}

</Content>
  <Actions>
    <Button on:click={() => (open = false)}>
      <Label>Cancel</Label>
    </Button>
  </Actions>
</Dialog>

