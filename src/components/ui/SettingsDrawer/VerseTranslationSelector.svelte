<script>
	import Checkbox from '$ui/FlowbiteSvelte/forms/Checkbox.svelte';
	import Check from '$svgs/Check.svelte';
	import { __verseTranslations } from '$utils/stores';
	import { selectableVerseTranslations, verseTranslationsLanguages } from '$data/options';
	import { updateSettings } from '$utils/updateSettings';
	import { selectedRadioClasses } from '$data/commonClasses';
</script>

<div class="grid gap-3 w-full theme-grayscale">
	{#each Object.entries(verseTranslationsLanguages) as [id, language]}
		{#if language.language_id !== 11115}
			<div class="flex flex-col space-y-2 pb-6">
				<div id="translation-name" class="text-md font-medium">{language.language}</div>
				<div id="translation-list" class="space-y-3">
					{#each Object.values(selectableVerseTranslations) as translation}
						{#if translation.language_id === language.language_id}
							<div class="flex items-center w-full">
								<Checkbox on:click={() => updateSettings({ type: 'verseTranslation', value: translation.resource_id })} custom>
									<div class="inline-flex justify-between items-center p-5 w-full text-gray-500 bg-white rounded-lg border border-gray-200 cursor-pointer peer-checked:border-primary-600 peer-checked:text-primary-600 hover:text-gray-600 hover:bg-gray-100 {$__verseTranslations.includes(translation.resource_id) && selectedRadioClasses}">
										<div class="w-full">{translation.resource_name}</div>

										{#if $__verseTranslations.includes(translation.resource_id)}
											<Check size={5} />
										{/if}
									</div>
								</Checkbox>
							</div>
						{/if}
					{/each}
				</div>
			</div>
		{/if}
	{/each}
</div>
