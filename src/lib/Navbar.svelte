<script lang="ts">
    import LayoutGrid, { Cell } from '@smui/layout-grid';
    import Button, { Label, Icon } from '@smui/button';
    import TimelineDialog from './dialogs/TimelineDialog.svelte';
    const SIMPLE_SIGNER_URL = import.meta.env.VITE_SIMPLE_SIGNER_URL

    const login = (async () => {
        window.open(
            `${SIMPLE_SIGNER_URL}/connect`,
            'Connect_Window',
            'width=360, height=450',
        );
    })

    const open_medium = (async () => {
        window.open("https://medium.com/stellar-community/hot-potato-having-fun-on-the-stellar-blockchain-63a5038fedc", '_blank').focus();
    })

    let open_timeline = false
    const openTimeline = function(){
        open_timeline = true
    }
</script>

<nav>
    <LayoutGrid>
        <Cell span={6} align="top">
            <h1>🥔 | Stellar Hot Potato</h1>
        </Cell>
        <Cell span={6} align="top">
            <Button variant="unelevated" class="button-shaped-round" on:click={openTimeline}>
                <Icon class="material-icons">view_timeline</Icon>
                <Label>Timeline</Label>
            </Button>
            <Button variant="unelevated" class="button-shaped-round" on:click={openTimeline}>
                <Icon class="material-icons">school</Icon>
                <Label>Learn</Label>
            </Button>
            <Button variant="outlined" class="button-shaped-round" on:click={login}>
                <Icon class="material-icons">login</Icon>
                <Label>Login</Label>
            </Button>
        </Cell>
    </LayoutGrid>
</nav>

<TimelineDialog bind:open={open_timeline}></TimelineDialog>

<style>
    h1 {
        text-align: left;
        line-height: 0%;
        overflow-wrap: break-word;
    }
</style>