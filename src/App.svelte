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

		<footer>
			<span>Created By</span>
			<a href="https://berkinakkaya.github.io">Berkin AKKAYA</a>
		</footer>
	</main>
{/if}

<style lang="scss">
	main {
		animation: Reveal 1s;
		display: flex;
		flex-direction: column;
		align-items: center;
		overflow: hidden;
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

	footer {
		margin: 75px 0;

		span {
			color: #999;
		}
		a {
			text-decoration: none;
			color: #005;
			display: inline-block;

			&::after {
				content: "";
				display: block;
				width: 0;
				height: 1px;
				background-color: #009;
				margin-top: 3px;
				transition: all 0.3s;
				position: relative;
				left: 50%;
				transform: translateX(-50%);
			}
			&:hover {
				color: #009;
			}
			&:hover::after {
				width: 100%;
			}
		}
	}
</style>
