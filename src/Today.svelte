<script>
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

		console.log(today);
	}

	function Toggle(habit) {
		const dt = currentDate;
		const td = new Date().getDate().toString();

		if (database[habit][dt].includes(td)) {
			database[habit][dt] = database[habit][dt].filter((x) => x != td);
		} else {
			database[habit][dt] = [...database[habit][dt], td];
		}
	}
</script>

<main>
	<h2>TODAY</h2>
	{#each today as habit}
		<div class="habit" class:done={habit[1]}>
			<input
				type="checkbox"
				on:click={() => Toggle(habit[0])}
				checked={habit[1]} />
			<span>{habit[0]}</span>
		</div>
	{/each}
</main>

<style lang="scss">
	main {
		margin-bottom: 50px;

		h2 {
			margin-bottom: 10px;
			text-align: center;
		}

		.habit {
			position: relative;
			display: grid;
			grid-template-columns: 14px 1fr;
			gap: 10px;
		}
		.habit::after {
			content: "";
			display: block;
			width: 0;
			height: 1px;
			background-color: black;
			position: absolute;
			top: 50%;
			left: 50%;
			transform: translate(-50%, -50%);
			transition: all 0.3s ease;
		}
		.habit.done {
			opacity: 0.5;

			&::after {
				width: calc(100% + 20px);
			}
		}
	}
</style>
