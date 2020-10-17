<script lang="ts">
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
				alert("TODO: Add " + typedHabit + " to the database.");
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
		}
		img {
			position: absolute;
			width: 30px;
			height: 30px;
			right: 0;
			top: 2px;
			opacity: 0.5;
			cursor: pointer;

			&:hover {
				opacity: 0.8;
			}
		}
	}
</style>
