<script lang="ts">
    import Dialog, { Title, Content, Actions } from '@smui/dialog';
    import Button, { Label } from '@smui/button';
    import Timeline from '../components/Timeline.svelte';

    export let open = false
</script>

<Dialog
  bind:open
  scrimClickAction=""
  escapeKeyAction=""
  aria-labelledby="mandatory-title"
  aria-describedby="mandatory-content"
  fullscreen
>
  <Title id="mandatory-title">Timeline</Title>
  <Content id="mandatory-content">
    <Timeline></Timeline>
  </Content>
  <Actions>
    <Button on:click={() => (open = false)}>
      <Label>Ok</Label>
    </Button>
  </Actions>
</Dialog>
 