<script>
    export const miliToTime = (duration) => {
        const milliseconds = Math.floor((duration % 1000) / 100)
        const seconds = Math.floor((duration / 1000) % 60)
        const minutes = Math.floor((duration / (1000 * 60)) % 60)
        const hours = Math.floor((duration / (1000 * 60 * 60)) % 24)
        
        return [hours, minutes, seconds]
    }
</script>