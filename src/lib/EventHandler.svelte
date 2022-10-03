<script lang="ts">
    import AccountNonExistent from "./dialogs/AccountNonExistent.svelte";
    import Holder from "./dialogs/Holder.svelte";
    import NotAHolder from "./dialogs/NotAHolder.svelte";
    import { claimable_xdr_signed } from './stores.js'

    export let original_xdr = undefined

    const SIMPLE_SIGNER_URL = import.meta.env.VITE_SIMPLE_SIGNER_URL

    const ISSUER = import.meta.env.VITE_ISSUER
    const HORIZON_URL = import.meta.env.VITE_HORIZON_URL
    const ASSET_CODE = import.meta.env.VITE_ASSET_CODE

    let open_wrong_key = false
    let open_success_dialog = false
    let error_dialog = false
    let claim_xdr_dialog = false
    let publicKey = ""
    let signedXDR = ""

    async function authenticate(key){
        const response = await fetch(`${HORIZON_URL}/accounts/${key}`)
        const data = await response.json()
        
        console.log(data)

        if(data['balances'] == undefined){
            error_dialog = true
            return
        }

        data['balances'].forEach(elem => {
            if(elem['asset_code'] !== undefined && elem['asset_code'] == ASSET_CODE && elem['asset_issuer'] == ISSUER){
                open_success_dialog = true
            } else {
                open_wrong_key = true
            }
        })

    }

    async function handleMessage(e) {
        // Reject messages that are not coming from simple signer (tailor this according to your needs)
        if (e.origin !== SIMPLE_SIGNER_URL) {
            return;
        }

        const messageEvent = e.data;
        if (messageEvent.type === 'onConnect') {
            publicKey = messageEvent.message.publicKey;
            await authenticate(publicKey)
        }

        if (messageEvent.type === 'onSign' && messageEvent.page === 'sign') {
            signedXDR = messageEvent.message.signedXDR;
        }

        
        if (original_xdr != undefined && messageEvent.type === 'onSign' && messageEvent.page === 'sign') {
            console.log(messageEvent.message.signedXDR)
            claimable_xdr_signed.set(messageEvent.message.signedXDR)
        }

    }
</script>

<svelte:window on:message={handleMessage}/>
<NotAHolder bind:open={open_wrong_key} publicKey={publicKey}></NotAHolder>
<Holder bind:open={open_success_dialog} bind:holder={publicKey} signedXDR={signedXDR}></Holder>
<AccountNonExistent bind:open={error_dialog} publicKey={publicKey}></AccountNonExistent>