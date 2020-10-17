<script lang="ts">
	import Day from "./Day.svelte";

	export let shown = {};
	export let fill = [];

	const Hide = () => (shown = {});
	const dayCount = new Date(shown["year"], shown["month"], 0).getDate();

	document.addEventListener("keydown", (e) => {
		e.key === "Escape" && Hide();
		e.key === "Backspace" && Hide();
	});
</script>

<main>
	<header>
		<p>{MonthNames[shown['month'] - 1]} {shown['year']}</p>
		<h1>{shown['habit']}</h1>
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
		justify-content: space-around;
		position: relative;
		text-align: center;

		@media (min-width: 600px) {
			justify-content: center;
		}

		p {
			position: absolute;
			left: 100px;
			font-size: 1.5em;
			opacity: 0.5;

			@media (max-width: 800px) {
				left: 50px;
			}
			@media (max-width: 600px) {
				display: none;
			}
		}

		h1::after {
			width: 100%;
			margin: 6px 0 0 0;
			position: relative;
			left: 50%;
			transform: translateX(-50%) rotate(-2deg);
		}

		span {
			right: 100px;
			font-size: 2em;
			opacity: 0.5;
			cursor: pointer;

			@media (min-width: 600px) {
				position: absolute;
			}
			@media (max-width: 800px) {
				right: 50px;
			}
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
