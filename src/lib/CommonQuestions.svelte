<script lang="ts">
    import LayoutGrid, { Cell } from '@smui/layout-grid';
</script>
<LayoutGrid>
    <Cell span={6}>
        <div class="question">
            <h3><u>How it works</u></h3>
            <p>One player has the hot potato. They have two choices. Pass the potato to someone else with a Stellar Account, or do nothing, and in 24 hours, the potato will disappear forever.</p>
        </div>
    </Cell>
    <Cell span={6}>
        <div class="question">
            <h3><u>How to play</u></h3>
            <p>If you own the hot potato, pass it to another user before it gets burned! You will need a Stellar wallet to interact with Hot Potato. Supported Wallets: ( Albedo, Freighter, Rabet, XBull, Pasting a secret key). Your task is to retrieve the destination's signature, provide your own and submit it to the network before the time expires.</p>
        </div>
    </Cell>
</LayoutGrid>