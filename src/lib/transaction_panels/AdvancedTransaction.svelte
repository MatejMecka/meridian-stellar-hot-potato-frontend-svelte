<script lang="ts">
    export let xdr
    export let text="send"
</script>

{#if text == "send"}
    <h4>This is the raw XDR that requires signatures from you and the destination key. It is your responsibility to sign the transaction and collect the destinations signature along with submitting it to the network.</h4>
{:else if text == "receive"}
    <h4>This is the raw signature that requires your signature. It is your responsibility to sign the transaction on time and submit it to the network.</h4>
{/if}
<code>{xdr.replace(' ', '+')}</code>

<h6>The following tools are available at your disposal:</h6>
<li>Stellar Laboratory</li>