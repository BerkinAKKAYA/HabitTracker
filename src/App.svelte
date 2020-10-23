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
		Test: {
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
	<MonthTracker
		bind:shown
		bind:fill={database[shown.habit][`${shown.month}-${shown.year}`]} />
{:else}
	<main>
		<h1>Habit Tracker</h1>

		<AddHabit bind:database {currentDate} />
		<Today bind:database {currentDate} />
		<HabitList bind:database bind:shown />
	</main>
{/if}

<style lang="scss">
	main {
		animation: Reveal 1s;
		display: flex;
		flex-direction: column;
		align-items: center;
		overflow: hidden;
		padding-bottom: 75px;
	}
	@keyframes Reveal {
		from {
			opacity: 0;
			height: 0;
		}
		to {
			opacity: 1;
			height: 100vh;
		}
	}

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
