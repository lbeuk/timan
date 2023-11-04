<script>
	import { onMount } from 'svelte';
	import { appWindow } from '@tauri-apps/api/window';
    import { XIcon } from 'svelte-feather-icons';

	onMount(() =>
		// @ts-ignore
		document.getElementById('titlebar-close').addEventListener('click', () => appWindow.close())
	);
</script>

<div data-tauri-drag-region class="titlebar">
    <div class="titlebar-right">
        <div class="window-actions">
            <div id="titlebar-close" role="button" tabindex="0">
                <XIcon size="15" />
            </div>
        </div>
    </div>
</div>

<style>
    .titlebar {
		display: flex;
		flex-direction: row;

		border-bottom: 1px solid var(--theme-bg-border-color);
	}

	.titlebar-right {
		margin-left: auto;
	}

	.window-actions > div {
		width: auto;
		display: inline-block;
		padding: 10px;
		line-height: 0;
	}

	#titlebar-close:hover {
		background-color: var(--theme-error-color);
	}
</style>