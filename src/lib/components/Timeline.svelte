<script lang="ts">
    import {
		Timeline,
		TimelineItem,
		TimelineSeparator,
		TimelineDot,
		TimelineConnector,
		TimelineContent,
        TimelineOppositeContent
	} from 'svelte-vertical-timeline';
    import CircularProgress from '@smui/circular-progress';

    const fetchTransactions = (async () => {
        const URL = `${import.meta.env.VITE_BACKEND_URL}/timeline`
		const response = await fetch(URL)
        return await response.json()
	})()

    // https://blog.webdevsimplified.com/2020-07/relative-time-format/
    const formatter = new Intl.RelativeTimeFormat(undefined, {numeric: 'auto', style:'long'})
    const DIVISIONS = [
        { amount: 60, name: 'seconds' },
        { amount: 60, name: 'minutes' },
        { amount: 24, name: 'hours' },
        { amount: 7, name: 'days' },
        { amount: 4.34524, name: 'weeks' },
        { amount: 12, name: 'months' },
        { amount: Number.POSITIVE_INFINITY, name: 'years' }
    ]

    function formatTimeAgo(date) {
        let duration = (new Date().getTime() - new Date(date).getTime()) / 1000

        for (let i = 0; i <= DIVISIONS.length; i++) {
            const division = DIVISIONS[i]
            if (Math.abs(duration) < division.amount) {
            return formatter.format(Math.round(duration), division.name)
            }
            duration /= division.amount
        }
    }

</script>

{#await fetchTransactions}
    <CircularProgress style="height: 32px; width: 32px;" indeterminate />
{:then data}
<Timeline>
	{#each data as option}
		<TimelineItem>
			<TimelineSeparator>
				<TimelineDot />
				<TimelineConnector />
			</TimelineSeparator>
			<TimelineContent>
				<h6>{option.buyer}</h6>
			</TimelineContent>
            <TimelineOppositeContent slot="opposite-content">
				<p>[PLACEHOLDER DATE]</p>
			</TimelineOppositeContent>
		</TimelineItem>
	{/each}
</Timeline>
{:catch error}
	<p>An error occurred!</p>
{/await}

