<script lang="ts">
	export let fill = "";
	export let day = 0;

	$: done = fill.split(",").includes((day + 1).toString());

	const ToggleDone = (key) => {
		let arr = fill.split(",") as Array<string>; // "Fill" Array
		const day = (key + 1).toString(); // Day Number instead of Index
		const index = arr.indexOf(day);

		if (index >= 0) {
			arr[index] = "";
		} else {
			arr.push(day);
		}

		arr = arr.filter((x) => !!x); // Filter Falsy Values
		fill = arr.join(",");
	};
</script>

<div class="day" class:done on:click={() => ToggleDone(day)}>{day + 1}</div>

<style lang="scss">
	.day {
		width: 100%;
		padding: 10px;
		background-color: white;
		box-shadow: inset 0 0 4px #eee;
		font-weight: bold;
		border: 1px solid var(--border-color);
		margin: -1px;
		opacity: 0.8;
		position: relative;
		user-select: none;
		cursor: pointer;

		@media (max-width: 500px) {
			padding: 20px;
		}

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

				@media (max-width: 600px) {
					width: 45px;
					height: 7px;
				}
			}
			&::after {
				width: 10px;
				height: 75px;

				@media (max-width: 600px) {
					width: 7px;
					height: 45px;
				}
			}
		}
	}
</style>
