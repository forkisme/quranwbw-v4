<script>
	export let key, page;

	import Tooltip from '$ui/FlowbiteSvelte/tooltip/Tooltip.svelte';
	import { pageNumberKeys } from '$data/quranMeta';
	import { __currentPage, __displayType } from '$utils/stores';
	import { selectableDisplays } from '$data/options';

	const dividerClasses = `
		flex flex-col text-center mx-auto w-full mt-8 
		${selectableDisplays[`${$__displayType}`].continuous ? 'mb-4' : 'mb-1'} 
		py-2 px-4 text-sm cursor-pointer rounded-full bg-lightGray theme
	`;
</script>

<!-- if the current key is the first verse of a page  -->
{#if $__currentPage === 'chapter'}
	{#if pageNumberKeys.includes(key)}
		<div class={dividerClasses}>Page {page}</div>
		<Tooltip arrow={false} type="light" placement="top" class="z-30 theme-grayscale font-normal">Start of Page {page}</Tooltip>
	{/if}
{/if}
