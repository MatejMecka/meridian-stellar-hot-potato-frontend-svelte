<script lang="ts">
    import LayoutGrid, { Cell } from '@smui/layout-grid';
    import Button, { Label, Icon } from '@smui/button';
    const SIMPLE_SIGNER_URL = import.meta.env.VITE_SIMPLE_SIGNER_URL

    const login = (async () => {
        window.open(
            `${SIMPLE_SIGNER_URL}/connect`,
            'Connect_Window',
            'width=360, height=450',
        );
    })
</script>

<nav>
    <LayoutGrid>
        <Cell span={6} align="top">
            <h1>🥔 | Stellar Hot Potato</h1>
        </Cell>
        <Cell span={6} align="top">
            <Button variant="unelevated" class="button-shaped-round">
                <Icon class="material-icons">view_timeline</Icon>
                <Label>Timeline</Label>
            </Button>
            <Button variant="unelevated" class="button-shaped-round">
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

<style>
    h1 {
        text-align: left;
    }
</style>