<script>
	import Checkbox from "./Checkbox.svelte";

	export let database = {};
	export let currentDate = "01-2000";

	let today = [];

	$: {
		today = [];
		for (const data of Object.keys(database)) {
			const date = new Date().getDate().toString();
			const monthData = database[data][currentDate];

			today.push([data, monthData.includes(date)]);
		}
		today = today;
	}

	function Toggle(habit) {
		const today = new Date().getDate().toString();
		let data = database[habit][currentDate];

		if (data.includes(today)) {
			data = data.filter((x) => x != today);
		} else {
			data = [...data, today];
		}

		database[habit][currentDate] = data;
	}
</script>

<main>
	<h2>TODAY</h2>
	{#each today as habit}
		<Checkbox
			task={habit[0]}
			bind:checked={habit[1]}
			onToggle={() => Toggle(habit[0])} />
	{/each}
</main>

<style lang="scss">
	main {
		margin-bottom: 50px;

		h2 {
			margin-bottom: 15px;
			text-align: center;
		}
	}
</style>
