<script lang="ts">
	import AddHabit from "./AddHabit.svelte";
	import HabitList from "./HabitList.svelte";
	import MonthTracker from "./Tracker/MonthTracker.svelte";
	import Today from "./Today.svelte";

	let currentDate = "";

	(function () {
		const today = new Date();
		const month = today.getMonth() + 1;
		const year = today.getFullYear();
		currentDate = `${month}-${year}`;
	})();

	let database = {
		Workout: {
			"10-2020": [],
			"11-2020": [],
		},
	};

	let shown = {
		habit: "",
		month: 0,
		year: 0,
	};
</script>

{#if shown.habit}
	<MonthTracker
		bind:shown
		bind:fill={database[shown.habit][`${shown.month}-${shown.year}`]} />
{:else}
	<h1>Habit Tracker</h1>

	<AddHabit bind:database {currentDate} />
	<Today bind:database {currentDate} />
	<HabitList bind:database bind:shown />
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
