<script lang="ts">
import SvelteSeo from "svelte-seo";
import EventHandler from '../lib/EventHandler.svelte';
import CommonQuestions from '../lib/CommonQuestions.svelte';
import Footer from '../lib/Footer.svelte';
import Navbar from '../lib/Navbar.svelte';
import Ticker from '../lib/Ticker.svelte'
import SignXdr from '../lib/dialogs/SignXDR.svelte';

const params = new Proxy(new URLSearchParams(window.location.search), {
  get: (searchParams, prop) => searchParams.get(prop),
});

let xdr = params.xdr
let status = true ? xdr != undefined : false
let url = window.location

</script>

<SvelteSeo
    openGraph={{
        title:"Stellar Hot Potato",
        description:"Stellar Hot Potato is an interactive game where the goal is to keep a potato alive instead of a tamagotchi.",
        url: url,
        type: 'website',
    }}
  />

<Navbar></Navbar>
<Ticker></Ticker>
<CommonQuestions></CommonQuestions>
<Footer></Footer>
<EventHandler bind:original_xdr={xdr}></EventHandler>
<SignXdr bind:xdr_raw={xdr} bind:open={status}></SignXdr>