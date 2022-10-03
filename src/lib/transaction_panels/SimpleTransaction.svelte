<script lang=ts>
    import Button, {Icon, Label} from '@smui/button';
    import IconButton from '@smui/icon-button';
    import CopyButton from '../components/CopyButton.svelte';

    export let xdr
    let snackbar_message;

    let shareData = {
        "title": "Hot Potato | Accept my Potato!",
        "text": "I'm sending you a potato to claim!",
        "url": `${window.location}`
    }

    async function share() {
        try{
            await navigator.share(shareData);
            snackbar_message = "Shared with Share Dialog!"
        } catch {
            snackbar_message = "There was an Error while attempting to share!"
        }
    }
    
</script>

<h4>You can allow Hot Potato to handle the collection of signatures. All you need to do is share the unique generated URL to the receiver, for them to redeem the potato</h4>

<CopyButton bind:xdr={xdr}></CopyButton>

{#if navigator.canShare}

<Button
  on:click={share}
  variant="unelevated"
  class="button-shaped-round"
>
  <Icon class="material-icons">share</Icon>
  <Label>Share</Label>
</Button>

{/if}