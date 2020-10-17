<script lang="ts">
	export let database = {};
	export let shown = {};

	// 10-2020 to October 2020
	function StringifyDate(date) {
		const split = date.split("-");
		const month = MonthNames[split[0] - 1];
		return `${month} ${split[1]}`;
	}

	function FillPercent(date, array) {
		const month = date.split("-")[0] - 1;
		const year = date.split("-")[1];
		const dayCount = new Date(year, month, 0).getDate();
		const fill = array.length * 100;
		return `${parseInt(fill / dayCount)}%`;
	}

	function Show(habit, date) {
		shown["habit"] = habit;
		shown["month"] = date.split("-")[0];
		shown["year"] = date.split("-")[1];
	}
</script>

<main>
	{#each Object.entries(database) as [habit, data]}
		<div class="habit">
			<h2>{habit}</h2>
			<p class="months">
				{#each Object.entries(data) as [date, fillArray]}
					<p class="month" on:click={() => Show(habit, date)}>
						{StringifyDate(date)}
						({FillPercent(date, fillArray)})
					</p>
				{/each}
			</p>
		</div>
	{/each}
</main>

<style lang="scss">
	main {
		.habit {
			text-align: center;

			&:not(:nth-child(1)) {
				margin-top: 50px;
			}

			.month {
				font-size: 1.1em;
				margin-top: 12px;
				opacity: 0.75;
				cursor: pointer;

				&:hover {
					opacity: 1;
				}
			}
		}
	}
</style>
