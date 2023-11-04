<script lang="ts">
	let months = [
		'January',
		'February',
		'March',
		'April',
		'May',
		'June',
		'July',
		'August',
		'September',
		'October',
		'November',
		'December'
	];

	let selected_date = new Date();
	let selected_month = selected_date.getMonth();
	let selected_year = selected_date.getFullYear();

	interface CalendarDate {
		day: number;
		selected: boolean;
	}

	function are_dates_same(date1: Date, date2: Date): boolean {
		return (
			date1.getFullYear() === date2.getFullYear() &&
			date1.getMonth() === date2.getMonth() &&
			date1.getDate() === date2.getDate()
		);
	}

	function calendar_data(month: number, year: number): (null | CalendarDate)[][] {
		let month_first_day = new Date(year, month, 1);
		let month_last_day = new Date(year, month + 1, 0).getDate();

		let first_day_of_week = month_first_day.getDay();
		console.log(month_first_day);
		console.log(first_day_of_week);

		let calendar_arr: (null | CalendarDate)[][] = [];

		// Fills in the calendar with prefixed null blocks
		for (let i = 0; i < first_day_of_week + month_last_day; i++) {
			let text: string = String(i - first_day_of_week + 1);
			if (i < first_day_of_week) {
				text = '';
			}

			let week_no = Math.floor(i / 7);
			let day_no = i % 7;

			if (day_no == 0) {
				calendar_arr.push([]);
			}

			calendar_arr[week_no].push(
				text.length > 0
					? {
							day: parseInt(text),
							selected: are_dates_same(
								new Date(selected_year, selected_month, parseInt(text)),
								selected_date
							)
					  }
					: null
			);
		}

		for (let i = 0; i < 7 - ((first_day_of_week + month_last_day) % 7); i++) {
			calendar_arr[calendar_arr.length - 1].push(null);
		}

		return calendar_arr;
	}

	function select_date(date: number) {
		selected_date = new Date(selected_year, selected_month, date);
		console.log(selected_date);
	}

	function sanitize_year(e: Event) {
		e.preventDefault();
		// @ts-ignore
		let val = parseInt(e.target.value);
		if (!isNaN(val)) {
			selected_year = val;
		}
	}
</script>

<div class="month-calendar">
	<input
		type="text"
		name="year"
		class="month-calendar-form"
		value={selected_year}
		on:input={sanitize_year}
	/>
	<select name="month" class="month-calendar-form" bind:value={selected_month}>
		{#each { length: 12 } as _, month}
			{#if month == selected_month}
				<option value={month} class="month-calendar-form" selected>{months[month]}</option>
			{:else}
				<option value={month} class="month-calendar-form">{months[month]}</option>
			{/if}
		{/each}
	</select>

	<div class="month-calendar-week">
		<div><span>S</span></div>
		<div><span>M</span></div>
		<div><span>T</span></div>
		<div><span>W</span></div>
		<div><span>R</span></div>
		<div><span>F</span></div>
		<div><span>S</span></div>
	</div>
	{#key selected_date}
		{#each calendar_data(selected_month, selected_year) as w}
			<div class="month-calendar-week">
				{#each w as day}
					<div>
						{#if day}
							<button
								class="month-calendar-day"
								class:selected={day.selected}
								on:click={select_date(day.day)}>{day.day}</button
							>
						{/if}
					</div>
				{/each}
			</div>
		{/each}
	{/key}
</div>

<style>
	.month-calendar {
		background-color: var(--theme-mg-color);
		padding: 10px;
		border-radius: 10px;
		display: flex;
		flex-direction: column;
		border: solid 1px var(--theme-border-color);
	}

	.month-calendar-form {
		background-color: inherit;
		color: inherit;
		border: none;
	}

	select {
		-webkit-appearance: none;
		color: inherit;
		border: none;
	}

	.month-calendar-week:nth-child(1) {
		border-bottom: 1px dashed var(--theme-mg-border-color);
	}

	.month-calendar-week {
		display: flex;
		flex-direction: row;
	}

	.month-calendar-week div {
		display: block;
		flex: 1;
		height: 32px;
		padding: 2px;
	}
	.month-calendar-week button, .month-calendar-week span {
		display: block;
		background-color: inherit;
		color: inherit;
		border: none;
		width: 100%;
		height: 100%;
		text-align: center;
		border-radius: 10px;
		line-height: 1.8;
	}

	.month-calendar-week button.selected {
		border: 1px solid;
	}

	.month-calendar-day:hover {
		background-color: var(--theme-mg-highlighted-color);
	}
</style>
