<script>
	export let data;

	import PageHead from '$misc/PageHead.svelte';
	import Spinner from '$svgs/Spinner.svelte';
	import WordsBlock from '$display/verses/WordsBlock.svelte';
	import Table from '$display/morphology/Table.svelte';
	import { quranMetaData } from '$data/quranMeta';
	import { apiEndpoint, errorLoadingDataMessage } from '$data/websiteSettings';
	import { __currentPage, __fontType, __wordTranslation, __morphologyKey, __pageURL, __displayType, __lexiconModalVisible, __wordRoot } from '$utils/stores';
	import { buttonOutlineClasses } from '$data/commonClasses';
	import { fetchVersesData } from '$utils/fetchData';
	import { term } from '$utils/terminologies';

	let fetchWordsData, fetchWordsData1, fetchWordSummary;
	let chapter, verse, word;

	// Split the key to get chapter, verse, and word numbers
	$: {
		const keySplit = data.key.split(':');
		chapter = +keySplit[0];
		verse = +keySplit[1];
		word = keySplit.length === 2 ? 1 : +keySplit[2];

		if (isNaN(word)) word = 1;

		__morphologyKey.set(`${chapter}:${verse}:${word}`);
	}

	// Fetch verse data based on chapter and verse
	$: fetchData = fetchVersesData(`${chapter}:${verse}`, $__fontType, $__wordTranslation);

	// Fetch words data for morphology
	$: {
		fetchWordsData = (async () => {
			try {
				const response = await fetch(`${apiEndpoint}/morphology?words=${$__morphologyKey}&word_translation=${$__wordTranslation}`);
				const data = await response.json();
				fetchWordsData1 = data.data;
				return data.data;
			} catch (error) {
				console.error(errorLoadingDataMessage, error);
				return [];
			}
		})();

		// Fetch word summary data
		fetchWordSummary = (async () => {
			try {
				const response = await fetch(`${apiEndpoint}/morphology/summary?word=${$__morphologyKey}&version=2`);
				const data = await response.json();
				return data.data;
			} catch (error) {
				console.error(errorLoadingDataMessage, error);
				return {};
			}
		})();
	}

	// Set the word root and show the lexicon modal
	function showLexiconModal() {
		const root = fetchWordsData1?.[0]?.morphology?.root?.root;
		if (root) {
			__wordRoot.set(root);
			__lexiconModalVisible.set(true);
		}
	}

	// Restrict display types to 1 or 2
	if (![1, 2].includes($__displayType)) $__displayType = 1;

	// Set the current page to 'morphology'
	__currentPage.set('morphology');
</script>

<PageHead title={`Morphology ${$__morphologyKey}`} />

<div class="space-y-12 my-8">
	<div id="verse-navigator" class="flex flex-row justify-center space-x-8 text-sm theme">
		<!-- previous chapter -->
		{#if verse === 1 && chapter > 1}
			<a href="/morphology/{+chapter - 1}:1" class={buttonOutlineClasses}>{@html '&#x2190;'} {term('chapter')} {+chapter - 1}</a>
		{/if}

		<!-- next verse -->
		{#if verse > 1}
			<a href="/morphology/{chapter}:{+verse - 1}" class={buttonOutlineClasses}>{@html '&#x2190;'} {term('verse')} {chapter}:{+verse - 1}</a>
		{/if}

		<!-- previous verse -->
		{#if verse < quranMetaData[chapter].verses}
			<a href="/morphology/{chapter}:{+verse + 1}" class={buttonOutlineClasses}>{term('verse')} {chapter}:{+verse + 1} {@html '&#x2192;'}</a>
		{/if}

		<!-- next chapter -->
		{#if verse === quranMetaData[chapter].verses && chapter < 114}
			<a href="/morphology/{+chapter + 1}:1" class={buttonOutlineClasses}>{term('chapter')} {+chapter + 1} {@html '&#x2192;'}</a>
		{/if}
	</div>

	<div id="verse">
		{#await fetchData}
			<Spinner />
		{:then fetchData}
			<div class="flex flex-wrap justify-center direction-rtl">
				{#each Object.entries(fetchData) as [key, value]}
					<WordsBlock {key} {value} />
				{/each}
			</div>
		{:catch error}
			<p>{errorLoadingDataMessage}</p>
		{/await}
	</div>

	<div id="word-summary" class="text-center opacity-70 mx-auto md:w-3/4 text-sm pb-6 border-b-2 border-black/10 md:text-lg theme">
		{#await fetchWordSummary}
			<span>...</span>
		{:then fetchWordSummary}
			<div class="flex flex-col space-y-4">
				<span>{@html fetchWordSummary.summary}</span>
				<!-- <button class="text-lg font-bold underline" on:click={() => showLexiconModal()}>View Lanes Lexicon Data &rarr;</button> -->
			</div>
		{:catch error}
			<p>{errorLoadingDataMessage}</p>
		{/await}
	</div>

	<div id="word-details" class="flex flex-col space-y-6">
		{#await fetchWordsData}
			<Spinner />
		{:then fetchWordsData}
			{#if !Object.values(fetchWordsData[0].morphology.verbs).every((o) => o === null)}
				<div id="word-forms" class="pb-8 border-b-2 border-black/10 theme">
					{#if Object.keys(fetchWordsData[0].morphology.root.words_with_same_root).length > 0}
						<div class="flex flex-col">
							<div id="different-verbs" class="theme-grayscale">
								<div class="mx-auto text-center">
									<div class="relative grid gap-8 grid-cols-2 row-gap-5 md:row-gap-8 md:grid-cols-6">
										{#each Object.entries(fetchWordsData[0].morphology.verbs) as [key, value]}
											{#if value !== null}
												<div class="flex flex-col py-5 duration-300 transform bg-white border rounded-3xl shadow-sm text-center hover:-translate-y-2">
													<div class="flex items-center justify-center mb-2">
														<p id="verb-1" class="text-xl md:text-2xl pb-4 leading-5 arabic-font-{$__fontType}">{value}</p>
													</div>
													<p class="text-xs text-gray-900 capitalize">{key.replace('_', ' ')}</p>
												</div>
											{/if}
										{/each}
									</div>
								</div>
							</div>
						</div>
					{:else}
						<div class="text-center my-8 text-sm opacity-70">Root data for this word is not available.</div>
					{/if}
				</div>
			{/if}

			<div id="word-root-data" class="pb-8 border-b-2 border-black/10 theme">
				<Table wordData={fetchWordsData[0].morphology.root.words_with_same_root} tableType={1} />
			</div>

			<div id="exact-word-data" class="pb-8 border-b-2 border-black/10 theme">
				<Table wordData={fetchWordsData[0].morphology.exact_words_in_quran} tableType={2} />
			</div>
		{:catch error}
			<p>{errorLoadingDataMessage}</p>
		{/await}
	</div>
</div>
