<script>
	import '../app.postcss';
	import { HomeIcon, CalendarIcon, SettingsIcon, XIcon } from 'svelte-feather-icons';
	import { appWindow } from '@tauri-apps/api/window';

</script>

<div data-tauri-drag-region class="titlebar flex flex-row text-zinc-400 bg-zinc-800">
	<div on:click={appWindow.close()} on:keydown={appWindow.close()} role="button" tabindex="0">
		<XIcon />
	</div>
</div>
<div class="flex flex-row">
	<div
		class="flex flex-col text-c h-screen p-2 space-y-2 border-r border-zinc-500 text-zinc-400 bg-zinc-800"
	>
		<a href="/"><HomeIcon /></a>
		<a href="/calendar"><CalendarIcon /></a>
		<div class="grow" />
		<a href=""><SettingsIcon /></a>
	</div>
	<div class="p-2">
		<slot />
	</div>
</div>

<style lang="postcss">
	:global(html) {
		background-color: theme(colors.zinc.900);
		color: theme(colors.zinc.50);
	}
</style>
