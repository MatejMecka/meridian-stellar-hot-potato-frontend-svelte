<script lang=ts>
    import Button, {Icon, Label} from '@smui/button';
    import IconButton from '@smui/icon-button';
    import CopyButton from '../components/CopyButton.svelte';
    import QRCode from '../components/QRJS.svelte';

    export let xdr
    let snackbar_message;
    let short_url = `${window.location}?xdr=${xdr}`


    let shareData = {
        "title": "Hot Potato | Accept my Potato!",
        "text": "I'm sending you a potato to claim!",
        "url": `${short_url}`
    }

    async function share() {
        try{
            await navigator.share(shareData);
            snackbar_message = "Shared with Share Dialog!"
        } catch {
            snackbar_message = "There was an Error while attempting to share!"
        }
    }

    const shortenUrl = async function (long_url) {
        return fetch(`https://api.shrtco.de/v2/shorten?url=${long_url}`)
            .then((res) => res.json())
            .then((data) => {
            console.log(data);
            short_url = data["result"]["full_short_link"]
            return data["result"]["full_short_link"];
            })
            .catch((err) => {
            console.error("URL Shortening failed... Defaulting to long url");
            console.error(err);
            return long_url;
            }); 
    };

    short_url = shortenUrl(`${window.location}?xdr=${xdr}`)
    
</script>

<h4>You can allow Hot Potato to handle the collection of signatures. All you need to do is share the unique generated URL to the receiver, for them to redeem the potato</h4>

{#await short_url then value}
<CopyButton bind:xdr={xdr} bind:shortened_url={short_url}></CopyButton>

{#if navigator.canShare}

<Button
  on:click={share}
  variant="unelevated"
  class="button-shaped-round"
>
  <Icon class="material-icons">share</Icon>
  <Label>Share</Label>
</Button>

{/if}

<h6>QR Code:</h6>
<QRCode codeValue={value} squareSize=200/>
{/await}