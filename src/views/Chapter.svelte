<script>
	// Import necessary components and utilities
	import PageHead from '$misc/PageHead.svelte';
	import Bismillah from '$display/Bismillah.svelte';
	import Chapter from '$display/verses/modes/Chapter.svelte';
	import Spinner from '$svgs/Spinner.svelte';
	import { parseURL } from '$utils/parseURL';
	import { fetchChapterData, fetchVerseTranslationData } from '$utils/fetchData';
	import { quranMetaData } from '$data/quranMeta';
	import { selectableDisplays } from '$data/options';
	import { errorLoadingDataMessage } from '$data/websiteSettings';
	import { __userSettings, __currentPage, __chapterNumber, __displayType, __fontType, __wordTranslation, __wordTransliteration, __verseTranslations, __pageURL, __firstVerseOnPage, __chapterDataLoaded, __verseTranslationData } from '$utils/stores';
	import { buttonOutlineClasses } from '$data/commonClasses';
	import { goto } from '$app/navigation';
	import { term } from '$utils/terminologies';

	// Export data and verse range variables
	export let data, startVerse, endVerse;

	// Maximum number of verses to load if total verses in chapter exceed this threshold
	const maxVersesThreshold = 5;

	let chapterData;

	// Fetch verses whenever there's a change in chapter or URL parameters
	$: {
		// Reset chapter data variables on chapter change
		resetChapterDataVariables(+data.chapter);

		// Update current chapter number
		__chapterNumber.set(+data.chapter);

		const chapterTotalVerses = quranMetaData[$__chapterNumber].verses;

		// Parse URL to get the range of verses to load
		[startVerse, endVerse] = parseURL();

		// If no specific range is selected, load verses from 1 to threshold or total chapter verses
		if (startVerse === 1 && endVerse === chapterTotalVerses) {
			startVerse = 1;
			endVerse = chapterTotalVerses > maxVersesThreshold ? maxVersesThreshold : chapterTotalVerses;
		}

		// Fetch chapter data from API
		chapterData = fetchChapterData($__chapterNumber);

		// Update the first verse on page
		__firstVerseOnPage.set(startVerse);

		// Check for store updates (page URL, display type, font type, word translation, transliteration)
		if ($__pageURL || $__displayType || $__fontType || $__wordTranslation || $__wordTransliteration) {
			// Do nothing except re-run the block
		}
	}

	// Fetch verse translation data if necessary
	$: if ($__verseTranslations) {
		fetchVerseTranslationData($__chapterNumber);
	}

	// Update the layout for the previous/next verse buttons
	$: loadPrevNextVerseButtons = `flex ${selectableDisplays[JSON.parse($__userSettings).displaySettings.displayType].continuous ? 'flex-row-reverse' : 'flex-row'} space-x-4 justify-center pt-8 pb-6 theme`;

	// Function to reset chapter data variables when chapter changes
	function resetChapterDataVariables(chapter) {
		if (chapter !== $__chapterNumber) {
			__chapterDataLoaded.set(false);
			localStorage.setItem('chapterDataLoaded', false);
		}
	}

	// Function to load the previous set of verses
	function loadPreviousVerse() {
		const versesOnPage = document.getElementsByClassName('verse');
		const firstVerseOnPage = +versesOnPage[0].id.split(':')[1];
		const lastVerseOnPage = +versesOnPage[versesOnPage.length - 1].id.split(':')[1];
		goto(`/${$__chapterNumber}/${+firstVerseOnPage - 1}-${+lastVerseOnPage}`, { replaceState: false });
	}

	// Set the current page to 'chapter'
	__currentPage.set('chapter');
</script>

<PageHead title={`${quranMetaData[$__chapterNumber].transliteration} (${$__chapterNumber})`} />

<div id="chapter-block">
	{#await chapterData}
		<Spinner height="screen" margin="-mt-20" />
	{:then}
		<Bismillah {startVerse} />

		<!-- need custom stylings if display type is 3 or 4 - continuous -->
		<div id="verses-block" class={selectableDisplays[JSON.parse($__userSettings).displaySettings.displayType].customClasses}>
			<!-- buttons to start chapter from start and load previous verse -->
			{#if startVerse > 1}
				<div class={loadPrevNextVerseButtons}>
					<a href="/{$__chapterNumber}" class="text-sm {buttonOutlineClasses}"> Start of {term('chapter')} </a>
					<button on:click={loadPreviousVerse} class="text-sm {buttonOutlineClasses}"> Previous {term('verse')} </button>
				</div>
			{/if}

			<Chapter {startVerse} {endVerse} />
		</div>
	{:catch error}
		<p>{errorLoadingDataMessage}</p>
	{/await}
</div>
