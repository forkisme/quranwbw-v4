<script>
	import Radio from '$ui/FlowbiteSvelte/forms/Radio.svelte';
	import Check from '$svgs/Check.svelte';
	import { __selectedDisplayId, __currentPage } from '$utils/stores';
	import { selectableDisplays } from '$data/options';
	import { selectedRadioClasses } from '$data/commonClasses';
	import { displayTypeChangeHandler } from '$utils/displayTypeChangeHandler';
</script>

<div class="grid gap-3 w-full theme-grayscale">
	{#each Object.entries(selectableDisplays) as [id, displayOption]}
		{#if !displayOption.disallowedIn.includes($__currentPage)}
			<Radio name="displayType" bind:group={$__selectedDisplayId} value={displayOption.displayID} on:change={(event) => displayTypeChangeHandler(+event.target.value)} custom>
				<div class="inline-flex justify-between items-center p-5 w-full text-gray-500 bg-white rounded-lg border border-gray-200 cursor-pointer peer-checked:border-primary-600 peer-checked:text-primary-600 hover:text-gray-600 hover:bg-gray-100 {$__selectedDisplayId === displayOption.displayID && selectedRadioClasses}">
					<div class="w-full">{displayOption.displayName}</div>

					{#if $__selectedDisplayId === displayOption.displayID}
						<Check size={5} />
					{/if}
				</div>
			</Radio>
		{/if}
	{/each}
</div>
