<script lang="ts">
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
		if (e.key.toLowerCase() === "s") {
			alert("TODO: Save On CTRL+S");
		}
	};

	const ToggleDone = (key) => {
		let day = (key + 1).toString();
		let arr = fill.split(",") as Array<string>;

		if (arr.includes(day)) {
			const index = arr.indexOf(day);
			delete arr[index];
		} else {
			arr.push(day);
		}

		arr = arr.filter((x) => !!x); // Filter Falsy Values
		fill = arr.join(",");
	};
</script>

<main>
	<header>
		<h1>{MonthNames[month - 1]}</h1>
		<span on:click={Hide}>X</span>
	</header>

	<div id="tracker">
		{#each [...Array(dayCount).keys()] as day}
			<div
				class="day {fill
					.split(',')
					.includes((day + 1).toString()) ? 'done' : ''}"
				on:click={() => ToggleDone(day)}>
				{day + 1}
			</div>
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
		display: grid;
		place-items: center;
		position: relative;

		h1::after {
			width: 100%;
			margin: 6px 0 0 0;
		}

		span {
			position: absolute;
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
		padding: 0 1px 1px 1px;

		@media (max-width: 1000px) {
			grid-template-columns: repeat(5, 1fr);
		}

		.day {
			padding: 10px;
			background-color: white;
			box-shadow: inset 0 0 4px #eee;
			font-weight: bold;
			border: 1px solid var(--border-color);
			margin: -1px;
			opacity: 0.8;
			position: relative;
			cursor: pointer;

			&:not(.done):hover {
				z-index: 1;
				box-shadow: inset 0 0 8px #d5d5d5;
			}

			&::before,
			&::after {
				width: 0;
				height: 0;
				border-radius: 100px;
				background-color: black;
				position: absolute;
				top: 50%;
				left: 50%;
				transform: translate(-50%, -50%) rotate(45deg);
				display: block;
				background-color: red;
				transition: all 0.3s ease;
				content: "";
			}

			&.done {
				z-index: 1;

				&:hover {
					box-shadow: inset 0 0 8px #ffaaaa;
				}

				&::before {
					width: 75px;
					height: 10px;
				}
				&::after {
					width: 10px;
					height: 75px;
				}
			}
		}
	}
</style>
