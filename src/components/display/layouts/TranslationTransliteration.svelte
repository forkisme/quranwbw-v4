<script>
	export let key, value;

	import VerseOptionButtons from '$display/verses/VerseOptionButtons.svelte';
	import WordsBlock from '$display/verses/WordsBlock.svelte';
	import Translations from '$display/verses/translations/Translations.svelte';
	import PageDivider from '$display/verses/PageDivider.svelte';
	import { updateSettings } from '$utils/updateSettings';
	import { inview } from 'svelte-inview';
</script>

{#if value}
	<!-- show page number  -->
	<PageDivider {key} page={value.meta.page} />

	<div id={key} data-words={value.meta.words} data-page={value.meta.page} data-juz={value.meta.juz} use:inview on:inview_enter={(event) => updateSettings({ type: 'lastRead', value: { key, page: value.meta.page } })} class="verse flex flex-col py-8 space-y-8 dark:border-slate-700">
		<VerseOptionButtons {key} {value} />

		<!-- words -->
		<div id="verse-{value.meta.verse}-words" class="flex flex-row-reverse flex-wrap hidden">
			<WordsBlock {key} {value} />
		</div>

		<!-- verse translations and transliterations -->
		<Translations {value} />
	</div>

	<div class="border-b border-black/10 theme"></div>
{/if}
