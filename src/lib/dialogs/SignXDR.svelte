<script lang="ts">
    import Dialog, { Title, Content, Actions } from '@smui/dialog';
    import Button, { Label } from '@smui/button';
    import Tab from '@smui/tab';
    import TabBar from '@smui/tab-bar';
    import AdvancedTransaction from '../transaction_panels/AdvancedTransaction.svelte';
    import SimpleClaim from '../transaction_panels/SimpleClaim.svelte';
    import Icon from '@smui/textfield/icon';
    import {claimable_xdr_signed} from '../stores.js';

    export let open
    export let xdr_raw
    let type_xdr = undefined
    

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

    let active = tabs[0]
    let data_type = "receive"

    claimable_xdr_signed.subscribe((value) => {
        xdr_raw = value != undefined ? value : xdr_raw
        type_xdr = value != undefined ? 'signed' : 'regular'
        console.log(type_xdr)
    })


</script>



<Dialog
  bind:open
  scrimClickAction=""
  escapeKeyAction=""
  aria-labelledby="mandatory-title"
  aria-describedby="mandatory-content"
  class="dialog_card"
>
  <Title id="mandatory-title">Claim a potato!</Title>
  <Content id="mandatory-content">
    You have received a transaction to claim a potato. A final signature is required from you to finalize the transfer.
    You have two options on how to achieve this:
    <br>
    <TabBar {tabs} let:tab bind:active>
        <Tab {tab}>
        <Icon class="material-icons">{tab.icon}</Icon>
        <Label>{tab.label}</Label>
        </Tab>
    </TabBar>

    {#if active == tabs[0]}
        <SimpleClaim bind:xdr={xdr_raw} bind:type={type_xdr}></SimpleClaim>
    {/if}

    {#if active == tabs[1]}
        <AdvancedTransaction bind:xdr={xdr_raw} bind:text={data_type}></AdvancedTransaction>
    {/if}


  </Content>
  <Actions>
    <Button on:click={() => (open = false)}>
      <Label>Ok</Label>
    </Button>
  </Actions>
</Dialog>

