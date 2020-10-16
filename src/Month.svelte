<script lang="ts">
	import Day from "./Day.svelte";

	export let month = 10;
	export let year = 2020;
	export let shownMonth = [];
	export let fill = "";

	const Hide = () => (shownMonth = []);
	const dayCount = new Date(year, month, 0).getDate();
	const MonthNames = [
		"January",
		"February",
		"March",
		"April",
		"May",
		"June",
		"July",
		"August",
		"September",
		"October",
		"November",
		"December",
	];
	let pressedCTRL = false;

	// KEY BINDINGS
	// ESC or Backspace for going back, CTRL+S for saving
	document.onkeydown = (e) => {
		e.key.toLowerCase() === "escape" && Hide();
		e.key.toLowerCase() === "backspace" && Hide();
		e.key.toLowerCase() === "s" && e.preventDefault();

		if (e.key === "Control") {
			pressedCTRL = true;
			e.preventDefault();
		}
	};
	document.onkeyup = (e) => {
		if (pressedCTRL && e.key.toLowerCase() === "s") {
			alert("TODO: Save On CTRL+S");
		}

		pressedCTRL = false;
	};
</script>

<main>
	<header>
		<h1>{MonthNames[month - 1]}</h1>
		<span on:click={Hide}>X</span>
	</header>

	<div id="tracker">
		{#each [...Array(dayCount).keys()] as day}
			<Day bind:fill {day} />
		{/each}
	</div>
</main>

<style lang="scss">
	main {
		height: 100vh;
		--border-color: #ddd;
	}

	header {
		height: 12vh;
		border-bottom: 1px solid var(--border-color);
		display: flex;
		align-items: center;
		justify-content: center;
		position: relative;

		@media (max-width: 500px) {
			justify-content: space-around;
		}

		h1::after {
			width: 100%;
			margin: 6px 0 0 0;
		}

		span {
			@media (min-width: 500px) {
				position: absolute;
			}

			right: 50px;
			font-size: 2em;
			opacity: 0.5;
			cursor: pointer;

			&:hover {
				opacity: 1;
			}
		}
	}

	#tracker {
		width: 100vw;
		height: 88vh;
		display: grid;
		grid-template-columns: repeat(7, 1fr);

		@media (max-width: 1000px) {
			grid-template-columns: repeat(5, 1fr);
		}
		@media (max-width: 500px) {
			grid-template-columns: 1fr 1fr;
		}
		@media (max-width: 300px) {
			grid-template-columns: 1fr;
		}
	}
</style>
