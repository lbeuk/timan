<script>
	import Titlebar from '$lib/Titlebar.svelte';
	import Sidebar from '$lib/Sidebar.svelte';
</script>

<div class="app">
	<Titlebar />
	<div class="app-body">
		<Sidebar />
		<slot />
	</div>
</div>

<style>
	:global(:root) {
	--theme-text-color: white;
	--theme-error-color: red;


    --theme-bg-color: hsl(0, 0%, 10%);
    --theme-bg-highlighted-color: hsl(0, 0%, 30%);
    --theme-bg-border-color: hsl(0, 0%, 25%);

	--theme-mg-color: hsl(0, 0%, 20%);
	--theme-mg-highlighted-color: hsl(0, 0%, 40%);
	--theme-mg-border-color: hsl(0, 0%, 35%);


}

:global(*) {
    -webkit-box-sizing: border-box;
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}

:global(body) {
    background-color: var(--theme-bg-color);
    color: var(--theme-text-color);
}

	.app {
		height: 100vh;
		display: flex;
		flex-direction: column;
	}

	.app-body {
		flex-grow: 1;
		display: flex;
		flex-direction: row;
	}
</style>