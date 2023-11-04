<script>
	import { HomeIcon, CalendarIcon, SettingsIcon, XIcon } from 'svelte-feather-icons';
</script>

<div class="sidebar">
	<a href="/"><HomeIcon /></a>
	<a href="/calendar"><CalendarIcon /></a>
	<div class="sidebar-bottom">
		<a href="/"><SettingsIcon /></a>
	</div>
</div>

<style>
    .sidebar {
		display: flex;
		flex-direction: column;
		padding: 5px;
		border-right: 1px solid var(--theme-bg-border-color);
	}

	.sidebar a {
		border-radius: 5px;
		color: inherit;
		padding: 7px;
		line-height: 0;
		display: block;
	}

	.sidebar a + a {
		margin-top: 5px;
	}

	.sidebar a:hover {
		background-color: var(--theme-bg-highlighted-color);
	}

	.sidebar-bottom {
        margin-top: auto;
    }
</style>