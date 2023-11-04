<script lang="ts">
	import { ChevronRightIcon, ChevronsRightIcon } from 'svelte-feather-icons';

	let use_24h = false;
	let hour_markers: String[] = [];
	let days = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday'];
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
	let hide_control_panel = false;

	for (let h = 0; h < 24; h++) {
		if (!use_24h) {
			let h_f = ((((h - 1) % 12) + 12) % 12) + 1; // Formats hours in 12 hour format
			let ampm = h / 12 > 1 ? 'AM' : 'PM';
			hour_markers.push(String(h_f) + ':00 ' + ampm);
		} else {
			hour_markers.push(String(h).padStart(2, '0') + ':00');
		}
	}

	function calendar_data(month: number, year: number): (null | String)[][] {
		let month_first_day = new Date(year, month, 1);
		let month_last_day = new Date(year, month + 1, 0).getDate();

		let first_day_of_week = month_first_day.getDay();
		console.log(month_first_day);
		console.log(first_day_of_week);

		let calendar_arr: (null | String)[][] = [];

		// Fills in the calendar with prefixed null blocks
		for (let i = 0; i < first_day_of_week + month_last_day; i++) {
			let text: null | String = String(i - first_day_of_week + 1);
			if (i < first_day_of_week) {
				text = null;
			}

			let week_no = Math.floor(i / 7);
			let day_no = i % 7;

			if (day_no == 0) {
				calendar_arr.push([]);
			}

			calendar_arr[week_no].push(text);
		}

		for (let i = 0; i < 7 - ((first_day_of_week + month_last_day) % 7); i++) {
			calendar_arr[calendar_arr.length - 1].push(null);
		}

		return calendar_arr;
	}

	function sanitize_year(e: Event) {
		e.preventDefault();
		let val = parseInt(e.target.value);
		if (!isNaN(val)) {
			selected_year = val;
		}
	}
</script>

<div class="calendar-page">
	<div class="calendar-control-pane-drawer-container">
		<button
			class="calendar-control-pane-toggle"
			class:hidden={hide_control_panel}
			on:click={() => (hide_control_panel = !hide_control_panel)}><ChevronsRightIcon /></button
		>
		<div class="calendar-control-pane-drawer" class:hidden={hide_control_panel}>
			<div class="calendar-control-pane">
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
					{#each calendar_data(selected_month, selected_year) as w}
						<div class="month-calendar-week">
							{#each w as day}
								<div>
									{#if day}
										<span class="month-calendar-day">{day}</span>
									{/if}
								</div>
							{/each}
						</div>
					{/each}
				</div>
			</div>
		</div>
	</div>

	<div class="calendar-content">Hello</div>
</div>

<style>
	.calendar-page {
		flex-grow: 1;
		display: flex;
		flex-direction: row-reverse;
	}

	.calendar-content {
		flex-grow: 1;
	}

	.calendar-control-pane-drawer-container {
		position: relative;
		padding: 10px 0;
	}

	.calendar-control-pane-toggle {
		background-color: var(--theme-mg-color);
		color: inherit;
		border: none;
		border-radius: 10px;

		position: absolute;
		width: 30px;
		height: 50px;
		left: -30px;
		transition: left 0.25s linear;

	}

	.calendar-control-pane-toggle.hidden {
		position: absolute;
		width: 30px;
		height: 50px;
		left: -40px;
	}

	:global(.calendar-control-pane-toggle.hidden > svg) {
		transform: scaleX(1);
	}

	:global(.calendar-control-pane-toggle.hidden > svg) {
		transform: scaleX(-1);
	}

	.calendar-control-pane-drawer {
		--calendar-control-pane-width: 300px;
		width: var(--calendar-control-pane-width);
		transition: width 0.25s linear;
		overflow-x: hidden;
	}

	.calendar-control-pane-drawer.hidden {
		width: 0;
	}

	.calendar-control-pane {
		width: var(--calendar-control-pane-width);
		display: flex;
		flex-direction: column;
		padding: 0 10px;
	}

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
	.month-calendar-week span {
		display: block;
		width: 100%;
		height: 100%;
		text-align: center;
		border-radius: 10px;
		line-height: 1.8;
	}

	.month-calendar-day:hover {
		background-color: var(--theme-mg-highlighted-color);
	}
</style>
