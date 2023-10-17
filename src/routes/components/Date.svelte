<script lang="ts">
	import { addDays, format } from 'date-fns';

	let today = new Date();

	function getNext7Days(startingDate: Date) {
		let days = [];
		for (let i = 0; i < 6; i++) {
			days.push(addDays(startingDate, i));
		}
		return days;
	}
	let next7Days: Date[] = getNext7Days(today);
</script>

<main>
	<ul class="flex gap-2">
		{#each next7Days as day (day)}
			<li
				class={`grid justify-center  p-4 text-center rounded-full w-full ${
					format(day, 'yyyy-MM-dd') === format(today, 'yyyy-MM-dd')
						? 'bg-blue-950 text-white'
						: 'bg-white text-black'
				}`}
			>
				<span class="block">{format(day, 'iiiiii')}</span>
				<span class="block text-lg font-semibold">
					{format(day, 'd')}<sup> {format(day, 'do').replace(/\d+/g, '')}</sup>
				</span>
				<span>{format(day, 'MMM')}</span>
			</li>
		{/each}
	</ul>
</main>
