<script>
	export let wordData, tableType;

	import { __fontType } from '$utils/stores';
	import { buttonClasses } from '$data/commonClasses';
	import { term } from '$utils/terminologies';

	const tableTitles = {
		1: { title: 'Words in Quran having same root', error: 'Root data for this word is not available.' },
		2: { title: 'Exact words in Quran', error: 'Exact word data for this word is not available.' }
	};

	const params = new URLSearchParams(window.location.search);
	const loadAll = params.get('load_all') === 'true';
	const totalAvailableWords = Object.keys(wordData).length;
	const maxResultsToLoad = 50;
	let lastWordToLoad = calculateInitialLastWordToLoad(loadAll, totalAvailableWords, maxResultsToLoad);

	function calculateInitialLastWordToLoad(loadAll, totalAvailableWords, maxResultsToLoad) {
		return loadAll ? totalAvailableWords : Math.min(totalAvailableWords, maxResultsToLoad);
	}

	function updateLastWordToLoad() {
		lastWordToLoad = Math.min(lastWordToLoad + 50, totalAvailableWords);
	}
</script>

{#if totalAvailableWords > 0}
	<div class="flex flex-col">
		<div class="relative space-y-6 sm:rounded-3xl theme-grayscale">
			<h1 class="text-md md:text-2xl text-center opacity-70">{tableTitles[tableType].title} ({totalAvailableWords})</h1>
			<div class="max-h-[32em] overflow-auto">
				<table class="w-full text-sm text-left rtl:text-right text-gray-500 rounded-md">
					<thead class="text-xs text-gray-700 uppercase bg-lightGray sticky top-0">
						<tr>
							<th scope="col" class="px-6 py-3"> # </th>
							<th scope="col" class="px-6 py-3"> Word </th>
							<th scope="col" class="px-6 py-3"> Translation </th>
							<th scope="col" class="px-6 py-3"> Transliteration </th>
							<th scope="col" class="px-6 py-3"> {term('verse')} </th>
							<th scope="col" class="px-6 py-3"> Word </th>
						</tr>
					</thead>
					<tbody>
						{#each Array.from(Array(lastWordToLoad + 1).keys()).slice(1) as word}
							<tr class="bg-white border-b dark:bg-gray-800 dark:border-gray-700 hover:bg-gray-50 dark:hover:bg-lightGray">
								<td class="px-6 py-4"> {word} </td>
								<td class="px-6 py-4 arabic-font-{$__fontType} text-xl md:text-2xl"> {wordData[word - 1].arabic} </td>
								<td class="px-6 py-4"> {wordData[word - 1].translation} </td>
								<td class="px-6 py-4"> {wordData[word - 1].transliteration} </td>
								<td class="px-6 py-4"> <a class="underline" href="/{wordData[word - 1].key.split(':')[0]}/{wordData[word - 1].key.split(':')[1]}">{wordData[word - 1].key.split(':')[0]}:{wordData[word - 1].key.split(':')[1]}</a> </td>
								<td class="px-6 py-4"> <a class="underline" href="/morphology/{wordData[word - 1].key}">{wordData[word - 1].key}</a> </td>
							</tr>
						{/each}
					</tbody>
				</table>
			</div>

			<!-- button to load more words -->
			{#if totalAvailableWords > maxResultsToLoad}
				<div class="text-center text-xs {lastWordToLoad === totalAvailableWords && 'hidden'}">
					<button on:click={() => updateLastWordToLoad()} class={buttonClasses}>Load more</button>
				</div>
			{/if}
		</div>
	</div>
{:else}
	<div class="text-center my-8 text-sm opacity-70">{tableTitles[tableType].error}</div>
{/if}
