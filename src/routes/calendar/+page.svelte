<script lang="ts">
	import { ChevronRightIcon, ChevronsRightIcon } from 'svelte-feather-icons';
	import DateSelector from '$lib/DateSelector.svelte';

	let use_24h = false;
	let hour_markers: String[] = [];
	let days = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday'];
	

	
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
				<DateSelector />
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
</style>
