<script lang=ts>
    import Button, {Icon, Label} from '@smui/button';
    import IconButton from '@smui/icon-button';

    export let xdr
    let snackbar_message;

    let shareData = {
        "title": "Hot Potato | Accept my Potato!",
        "text": "",
        "url": ""
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

<Button
  on:click={() => {}}
  variant="unelevated"
  class="button-shaped-round"
>
  <Icon class="material-icons">content_copy</Icon>
  <Label>Copy to Clipboard</Label>
</Button>

{#if navigator.canShare}

<Button
  on:click={() => {}}
  variant="unelevated"
  class="button-shaped-round"
>
  <Icon class="material-icons">share</Icon>
  <Label>Share</Label>
</Button>

{/if}