<script lang="ts">
	import Month from "./Month.svelte";

	let shownMonth = [];
	let database = { "1-2020": "", "2-2020": "" };

	const GetDate = (dt) => {
		return {
			month: parseInt(dt.split("-")[0]),
			year: parseInt(dt.split("-")[1]),
		};
	};
	const AddMonth = () => {
		const month = parseInt(prompt("Month (1, 2, ..., 12)"));
		if (isNaN(month) || month < 1 || month > 12) {
			return alert("Invalid Month Number");
		}

		const year = parseInt(prompt("Year (2019, 2020...)"));
		if (isNaN(year) || year < 1900 || year > 2100) {
			return alert("Invalid Year");
		}

		const date = `${month}-${year}`;
		database[date] = "";
	};
</script>

{#if shownMonth.length !== 0}
	<Month
		bind:shownMonth
		bind:fill={database[shownMonth.join('-')]}
		month={shownMonth[0]}
		year={shownMonth[1]} />
{:else}
	<h1>Habit Tracker</h1>
	<div id="months">
		{#each Object.entries(database) as [dt, fill]}
			<div
				class="month"
				on:click={() => {
					shownMonth = [GetDate(dt).month, GetDate(dt).year];
				}}>
				<p class="date">{GetDate(dt).month}/{GetDate(dt).year}</p>
				<p class="fillCount">
					{fill ? fill.split(',').length : 0}
					out of
					{new Date(GetDate(dt).year, GetDate(dt).month, 0).getDate()}
				</p>
				<div class="percent">
					{parseInt(((fill ? fill.split(',').length : 0) * 100) / new Date(GetDate(dt).year, GetDate(dt).month, 0).getDate())}%
				</div>
			</div>
		{/each}
		<div class="add month" on:click={AddMonth}>ADD</div>
	</div>
{/if}

<style lang="scss">
	h1 {
		margin: 100px 0 75px 0;
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
	#months {
		width: 80%;
		padding: 10px;
		gap: 10px;
		display: grid;
		grid-template-columns: repeat(6, 1fr);
		align-items: center;

		@media (max-width: 1300px) {
			grid-template-columns: repeat(4, 1fr);
		}
		@media (max-width: 1000px) {
			grid-template-columns: repeat(3, 1fr);
		}
		@media (max-width: 700px) {
			width: 90%;
			grid-template-columns: repeat(2, 1fr);
		}
		@media (max-width: 500px) {
			width: 100%;
			grid-template-columns: repeat(1, 1fr);
		}

		.month {
			height: 250px;
			display: flex;
			align-items: center;
			justify-content: center;
			flex-direction: column;
			text-align: center;
			padding: 30px 15px;
			border-radius: 5px;
			background-color: #eee;
			box-shadow: 0 0 0 #fff;
			box-shadow: inset 0 0 0 #fff;
			border: 1px solid #aaa;
			cursor: pointer;

			.date,
			.fillCount {
				padding: 5px 0;
			}

			&:hover {
				* {
					transform: scale(1.2);
					margin: 5px 0;
				}
				box-shadow: inset 0 0 10px #ccc;
				border-radius: 25px;
			}
		}
		.add {
			font-weight: bold;
			font-size: 2em;
			border-radius: 40px;
			transform: scale(0.9);

			&:hover {
				border-radius: 40px;
				transform: scale(1);
			}
		}
	}
</style>
