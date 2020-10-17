<script lang="ts">
	import AddHabit from "./AddHabit.svelte";

	import HabitList from "./HabitList.svelte";
	import MonthTracker from "./Tracker/MonthTracker.svelte";

	let database = {
		"Workout": {
			"10-2020": [],
			"11-2020": [],
		},
		"Don't Eat Sugar": {
			"09-2020": [],
			"10-2020": [],
		},
	};

	let shown = {
		habit: "",
		month: 0,
		year: 0,
	};
</script>

{#if shown.habit}
	<MonthTracker {...shown} />
{:else}
	<h1>Habit Tracker</h1>

	<AddHabit />
	<HabitList />
{/if}

<style lang="scss">
	h1 {
		margin: 75px 0;
	}
	:global(h1) {
		color: #333;

		&::after {
			content: "";
			display: block;
			width: 80%;
			height: 3px;
			margin-left: 10%;
			margin-top: 10px;
			background-color: black;
			transform: rotate(-1deg);
			opacity: 0.7;
		}
	}
</style>
