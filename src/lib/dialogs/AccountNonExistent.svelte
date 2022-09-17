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
  <Title id="mandatory-title">The provided account does not exist!</Title>
  <Content id="mandatory-content">
    The account you provided does not exist on the Stellar Blockchain. All accounts on Stellar must have a minimum of 1 XLM in order to exist on the network. Provided public key: <b>{publicKey}</b>
  </Content>
  <Actions>
    <Button on:click={() => (open = false)}>
      <Label>Ok</Label>
    </Button>
  </Actions>
</Dialog>

