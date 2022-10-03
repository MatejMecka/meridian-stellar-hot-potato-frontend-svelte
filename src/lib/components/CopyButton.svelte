<script lang="ts">
    import Button, {Icon, Label} from '@smui/button';
    export let xdr = ""
    let copied = false 

    const copy = function() {
        const url = `${window.location}?xdr=${xdr}`
        // Copy the text inside the text field
        navigator.clipboard.writeText(url);
        copied = true
    }
</script>

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