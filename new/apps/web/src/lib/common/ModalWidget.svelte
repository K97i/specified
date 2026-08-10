<!-- 
 A widget that expands into a modal view/pop up when clicked.
-->
<script lang="ts">
	import { Dialog, Portal } from '@skeletonlabs/skeleton-svelte';

	interface Widget {
		title: string,
		widgetContents: any,
		modalContents?: any,
		extraModalContents?: any,
		fullScreen?: any,
	}

	let {
		title = 'Modal',
		/** what's displayed when the widget is not expanded */
		widgetContents,
		/** what's displayed when the widget is in modal mode*/
		modalContents,
		/** "more info" contents*/
		extraModalContents,
		fullScreen
	}: Widget = $props();

	// TODO: support for "more info" is not currently
	// implemented. When it is, it should not make use
	// of IDs
	let moreInfoExpanded = $state(false);

	const widget = 'w-60 max-w-80 grow bg-surface-900 p-1 rounded-4',
		  h1 = 'font-normal text-[13pt] m-0 pt-1 pb-2 text-center text-surface-50',
		  contents = 'flex flex-col grow text-center text-[1rem]',
		animation = 'transition transition-discrete translate-y-[100px] starting:data-[state=open]:opacity-0 starting:data-[state=open]:translate-y-[100px] data-[state=open]:opacity-100 data-[state=open]:translate-y-0';
</script>

{#if !modalContents}
<div class="{widget}">
	<h1 class="{h1}">{title}</h1>
	<div class="{contents}">
			{@render widgetContents()}
	</div>
</div>
{:else}
<Dialog>
	<Dialog.Trigger class="transition-all duration-100 hover:brightness-75">
		<div class="{widget}">
			<h1 class="{h1}">{title}</h1>
			<div class="{contents}">
					{@render widgetContents()}
			</div>
		</div>
	</Dialog.Trigger>
	<Portal>
		<Dialog.Backdrop class="fixed inset-0 z-50 bg-surface-50-950/50" />
		<Dialog.Positioner class="fixed inset-0 z-100 flex justify-center items-center p-4">
			<Dialog.Content class="card bg-surface-100-900 {fullScreen ? "w-full max-w-11/12 max-h-11/12" : "w-fit max-w-6xl max-h-4xl"} p-4 space-y-4 shadow-xl {animation}">
				<header class="flex justify-between items-center">
					<Dialog.Title class="text-lg font-bold">{title}</Dialog.Title>
					<Dialog.CloseTrigger class="btn-icon hover:preset-tonal">
						X
					</Dialog.CloseTrigger>
				</header>

				<div class="max-h-200 overflow-scroll">
					{@render modalContents()}

					<!-- more info -->	
					{#if extraModalContents && moreInfoExpanded}
						<div class="modal-body">
							{@render extraModalContents()}
						</div>
					{/if}
				</div>
				
				<footer class="flex justify-end gap-2">
					{#if extraModalContents && moreInfoExpanded == false}
						<button onclick={() => {moreInfoExpanded = true}} type="button" class="btn preset-filled">More Info</button>
					{/if}
					<Dialog.CloseTrigger class="btn preset-tonal">Close</Dialog.CloseTrigger>
				</footer>
			</Dialog.Content>
		</Dialog.Positioner>
	</Portal>
</Dialog>
{/if}


<style>

/*
Underscore needed to stop bootstrap from interfering with css
can be removed when bootstrap is
*/
</style>