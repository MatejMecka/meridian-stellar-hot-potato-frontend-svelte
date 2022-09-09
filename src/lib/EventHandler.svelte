<script lang="ts">
    const SIMPLE_SIGNER_URL = import.meta.env.VITE_SIMPLE_SIGNER_URL

    function handleMessage(e) {
        // Reject messages that are not coming from simple signer (tailor this according to your needs)
        if (e.origin !== SIMPLE_SIGNER_URL) {
            return;
        }

        const messageEvent = e.data;
        if (messageEvent.type === 'onConnect') {
            const publicKey = messageEvent.message.publicKey;
            console.log(publicKey)
        }
    }
</script>

<svelte:window on:message={handleMessage}/>