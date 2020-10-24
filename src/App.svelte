<script lang="ts">
	import AddHabit from "./AddHabit.svelte";
	import HabitList from "./HabitList.svelte";
	import MonthTracker from "./Tracker/MonthTracker.svelte";
	import Today from "./Today.svelte";
	import SignInPanel from "./SignInPanel.svelte";

	let currentDate = "";
	let loggedIn = "";

	(function () {
		const today = new Date();
		const month = today.getMonth() + 1;
		const year = today.getFullYear();
		currentDate = `${month}-${year}`;
	})();

	let database = {};
	$: if (loggedIn && Object.keys(database).length > 0) {
		UpdateDatabase(loggedIn, database);

		console.log(database);
	}

	let shown = {
		habit: "",
		month: 0,
		year: 0,
	};

	auth.onAuthStateChanged((result) => {
		loggedIn = result ? result.uid : "";

		if (loggedIn) {
			const ref = realtimeDatabase.ref(loggedIn);

			ref.once("value").then((ss) => {
				const result = ss.val();
				database = result || {};

				// Convert arrays to strings
				for (const habit of Object.keys(database)) {
					for (const month of Object.keys(database[habit])) {
						let data = database[habit][month];
						data = data.split(",").filter((x) => !!x);
						database[habit][month] = data;
					}
				}
			});
		}
	});

	function Logout() {
		auth.signOut();
	}
</script>

{#if !shown.habit}
	<h1>Habit Tracker</h1>
{/if}

{#if loggedIn}
	<span id="logout" on:click={Logout}>Logout</span>

	{#if shown.habit}
		<MonthTracker
			bind:shown
			bind:fill={database[shown.habit][`${shown.month}-${shown.year}`]} />
	{:else}
		<main>
			<AddHabit bind:database {currentDate} uid={loggedIn} />
			<Today bind:database {currentDate} />
			<HabitList bind:database bind:shown />

			<footer>
				<span>Created By</span>
				<a href="https://berkinakkaya.github.io">Berkin AKKAYA</a>
			</footer>
		</main>
	{/if}
{:else}
	<SignInPanel bind:loggedIn />
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
		margin: 50px 0;
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

	#logout {
		margin-bottom: 25px;
		opacity: 0.5;
		color: #a00;
		cursor: pointer;

		&:hover {
			opacity: 1;
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
