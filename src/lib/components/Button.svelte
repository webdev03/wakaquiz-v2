<script lang="ts">
	export let goTo: string = '';
	export let type = 'link';
	export let fn: any = () => {};
</script>

{#if type == 'link'}
	<a
		class="bg-blue-300 p-2 rounded shadow-md hover:bg-blue-400 hover:shadow-lg dark:text-gray-900"
		href={goTo}
	>
		<slot />
	</a>
{:else}
	<button
		class="bg-blue-300 p-2 rounded shadow-md hover:bg-blue-400 hover:shadow-lg dark:text-gray-900"
		on:click={fn}
	>
		<slot />
	</button>
{/if}
