<script lang="ts">
    import Dialog, { Title, Content, Actions } from '@smui/dialog';
    import Button, { Label } from '@smui/button';

    export let open = false
    export let publicKey = ""
</script>

<Dialog
  bind:open
  scrimClickAction=""
  escapeKeyAction=""
  aria-labelledby="mandatory-title"
  aria-describedby="mandatory-content"
  class="dialog_card"
>
  <Title id="mandatory-title">You do not own a 🥔!</Title>
  <Content id="mandatory-content">
    You do not own a Hot Potato. Make sure you're using the correct public key. The provided public key was {publicKey}
  </Content>
  <Actions>
    <Button on:click={() => (open = false)}>
      <Label>Ok</Label>
    </Button>
  </Actions>
</Dialog>

