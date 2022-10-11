<script lang="ts">
    export let xdr
    export let text="send"
    export let copied = false

    import Button, {Icon, Label} from '@smui/button';

    const copy = function() {
        // Copy the text inside the text field
        navigator.clipboard.writeText(xdr);
        copied = true
    }

</script>

{#if text == "send"}
    <h4>This is the raw XDR that requires signatures from you and the destination key. It is your responsibility to sign the transaction and collect the destinations signature along with submitting it to the network.</h4>
{:else if text == "receive"}
    <h4>This is the raw signature that requires your signature. It is your responsibility to sign the transaction on time and submit it to the network.</h4>
{/if}
<code>{xdr.replace(' ', '+')}</code>

<h6>The following tools are available at your disposal:</h6>
<li>Stellar Laboratory</li>

<Button
  on:click={() => copy()}
  variant="unelevated"
  class="button-shaped-round"
>
  <Icon class="material-icons">content_copy</Icon>
  <Label>Copy to Clipboard</Label>
</Button>

{#if copied}
    <br><br>
    <b style="color:green">Copied to Clipboard!</b>
{/if}