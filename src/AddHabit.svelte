<script lang="ts">
	export let database = {};
	export let currentDate = "01-2000";

	let focused = false;
	let typedHabit = "";

	const Focus = () => (focused = true);
	const Blur = () => (focused = false);

	document.addEventListener("keydown", (e) => {
		if (e.key === "Enter" && focused) {
			Submit();
		}
	});

	function Submit() {
		if (typedHabit.length > 1) {
			const sure = confirm('Start the habit "' + typedHabit + '" ?');

			if (sure) {
				if (typedHabit in database) {
					alert("Habit already exists!");
				} else {
					database[typedHabit] = {};
					database[typedHabit][currentDate] = [];
				}
			}
		}
	}
</script>

<div id="addHabit">
	<input
		type="text"
		placeholder="Start a new habit..."
		on:focus={Focus}
		on:blur={Blur}
		maxlength="30"
		bind:value={typedHabit} />
	<img src="img/add.svg" alt="ADD" />
</div>

<style lang="scss">
	#addHabit {
		width: 400px;
		max-width: 90vw;
		position: relative;
		margin-bottom: 75px;

		input[type="text"] {
			background: none;
			border-bottom: 1px solid #ddd;
			padding: 10px 15px;
			font-size: 1.2em;
			width: 100%;
			color: #666;

			&::placeholder {
				color: #666 !important;
			}
			&:hover {
				border-color: #aaa;
			}
			&:focus {
				border-color: #666;
				color: #000;
			}
		}
		img {
			position: absolute;
			height: 75%;
			right: 0;
			top: 50%;
			transform: translateY(-50%);
			border-radius: 100%;
			background-color: white;
			opacity: 0.7;
			cursor: pointer;

			&:hover {
				opacity: 1;
			}
		}
	}
</style>
