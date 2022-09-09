<script lang="ts">
    import LayoutGrid, { Cell } from '@smui/layout-grid';
</script>
<LayoutGrid>
    <Cell span={6}>
        <div class="question">
            <h3><u>How it works</u></h3>
            <p>One player is in possession of the hot potato. They are presented with 2 choices. Pass the potato to someone else with a Stellar Account, or do nothing and in 24 hours the potato will dissapear forever</p>
        </div>
    </Cell>
    <Cell span={6}>
        <div class="question">
            <h3><u>How to play</u></h3>
            <p>If you are in possession of the hot potato, make sure to pass to another user before it gets burned! You will need a Albedo wallet to pass & receive. After passing the Potato, a Short URL will be generated to send to the new receiver and complete accepting the potato! </p>
        </div>
    </Cell>
</LayoutGrid>