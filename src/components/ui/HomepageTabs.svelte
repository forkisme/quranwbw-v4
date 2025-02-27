<script>
	import Mecca from '$svgs/Mecca.svelte';
	import Madinah from '$svgs/Madinah.svelte';
	import CrossSolid from '$svgs/CrossSolid.svelte';
	import AscendingSort from '$svgs/AscendingSort.svelte';
	import Tooltip from '$ui/FlowbiteSvelte/tooltip/Tooltip.svelte';
	import { updateSettings } from '$utils/updateSettings';
	import { quranMetaData, mostRead } from '$data/quranMeta';
	import { __lastRead, __favouriteChapters, __userBookmarks, __userNotes, __timeSpecificChapters, __siteNavigationModalVisible, __quranNavigationModalVisible } from '$utils/stores';
	import { buttonClasses, buttonOutlineClasses } from '$data/commonClasses';
	import { checkTimeSpecificChapters } from '$utils/checkTimeSpecificChapters';
	import { term } from '$utils/terminologies';
	import { fetchSingleVerseData } from '$utils/fetchData';
	import { splitDelimiter } from '$data/websiteSettings';

	// CSS classes for chapter cards and tabs
	const cardGridClasses = 'grid md:grid-cols-2 lg:grid-cols-3 gap-3';
	const cardInnerClasses = 'flex justify-between md:text-left border border-black/10 transition text-sm bg-gray-100 rounded-3xl p-5 hover:cursor-pointer hover:bg-lightGray';
	const tabClasses = 'p-2 md:p-3 text-xs md:text-md cursor-pointer border-b-4 border-transparent';
	const activeTabClasses = '!border-black/10';

	let lastReadChapter = 1;
	let lastReadVerse = 1;
	let activeTab = 1; // Default to chapters tab
	let chapterSortIsAscending = true;
	let chapterListOrder = [...quranMetaData];

	// Reactive variable to update last read chapter and verse
	$: {
		if ($__lastRead.hasOwnProperty('key')) {
			[lastReadChapter, lastReadVerse] = $__lastRead.key.split(':').map(Number);
		}
	}

	// Reactive variable to fetch bookmarks data when on the bookmarks tab
	$: fetchData = activeTab === 3 && totalBookmarks !== 0 ? fetchSingleVerseData($__userBookmarks.toString(), 1) : null;
	$: totalBookmarks = $__userBookmarks.length;
	$: totalNotes = Object.keys($__userNotes).length;

	// Remove 'invisible' class from chapter icons once fonts are loaded
	document.fonts.ready.then(() => {
		document.querySelectorAll('.chapter-icons').forEach((element) => {
			element.classList.remove('invisible');
		});
	});

	// Check if it's Friday and nighttime
	checkTimeSpecificChapters();

	// Function to sort the chapter list in ascending or descending order
	function sortChapters() {
		chapterSortIsAscending = !chapterSortIsAscending;
		chapterListOrder = chapterSortIsAscending ? [...quranMetaData] : [...quranMetaData].reverse();

		// Ensure chapter icons are visible after sorting
		setTimeout(() => {
			document.querySelectorAll('.chapter-icons').forEach((element) => {
				element.classList.remove('invisible');
			});
		}, 10);
	}
</script>

<div id="homepage-tabs" style="margin-top: 15px;" class="theme-grayscale">
	<div class="flex items-center justify-center px-2 mb-4">
		<!-- <div class="hidden md:block flex-1 border-t border-black/10"></div> -->

		<div class="flex flex-row justify-center px-4">
			<!-- main tabs -->
			<div id="tab-buttons">
				<div class="flex text-sm font-medium text-center opacity-70 justify-center space-x-1 md:space-x-4">
					<button on:click={() => (activeTab = 1)} class="{tabClasses} {activeTab === 1 && activeTabClasses} flex flex-row space-x-2 items-center" type="button">
						<!-- asc/dsc sort button -->
						<div class="flex flex-row">
							<button class="inline-flex p-2 rounded-full text-black items-center {chapterSortIsAscending ? 'bg-gray-200' : 'bg-gray-300'}" on:click={() => sortChapters()}>
								<span class="opacity-70"><AscendingSort size={3} /></span>
								<span class="sr-only">Sort</span>
							</button>
							<Tooltip arrow={false} type="light" placement="top" class="z-30 w-max hidden md:block font-filter font-normal">Sort Asc/Dsc</Tooltip>
						</div>
						<span>{term('chapters')}</span>
					</button>
					<button on:click={() => (activeTab = 2)} class="{tabClasses} {activeTab === 2 && activeTabClasses}" type="button">Suggested</button>
					<button on:click={() => (activeTab = 3)} class="{tabClasses} {activeTab === 3 && activeTabClasses} flex flex-row space-x-1 items-center truncate" type="button">
						<span>Bookmarks</span>
						<span class="hidden xs:block">{totalBookmarks > 0 ? `(${totalBookmarks})` : ''}</span>
					</button>
					<button on:click={() => (activeTab = 4)} class="{tabClasses} {activeTab === 4 && activeTabClasses} flex flex-row space-x-1 items-center truncate" type="button">
						<span>Notes</span>
						<span class="hidden xs:block">{totalNotes > 0 ? `(${totalNotes})` : ''}</span>
					</button>
				</div>
			</div>
		</div>
	</div>

	<div class="hidden md:block border-b -mt-4 px-4 mx-auto w-[98%] theme-grayscale"></div>

	<div id="content-tab" class="my-6 px-">
		<!-- chapters tab -->
		<div class="homepage-tab-panels {activeTab === 1 ? 'block' : 'hidden'}" id="chapters-tab-panel" role="tabpanel" aria-labelledby="chapters-tab">
			<!-- time specific chapters and continue reading button -->
			<div class="flex flex-row justify-between text-xs mb-2 space-x-1 md:space-x-2">
				<!-- time specific chapters buttons -->
				<!-- show Al Kahf on Friday -->
				{#if $__timeSpecificChapters.isFriday || $__timeSpecificChapters.isNight}
					<div class="flex flex-row space-x-1 md:space-x-2 w-full md:w-max">
						{#if $__timeSpecificChapters.isFriday}
							<div id="al-kahf" class="w-full md:w-max">
								<a href="/18" class="py-2.5 w-full truncate {buttonClasses}">
									<span class="hidden md:block">It's Friday:&nbsp;</span>
									Al-Kahf
									<span class="hidden md:block">{@html '&#10230'}</span>
								</a>
							</div>
						{/if}

						<!-- show Al Mulk at night/evening -->
						{#if $__timeSpecificChapters.isNight}
							<div id="al-mulk" class="w-full md:w-max">
								<a href="/67" class="py-2.5 w-full truncate {buttonClasses}">
									<span class="hidden md:block">Night Reminder:&nbsp;</span>
									Al-Mulk
									<span class="hidden md:block">{@html '&#10230'}</span>
								</a>
							</div>
						{/if}
					</div>
				{/if}

				<!-- continue reading button -->
				{#if $__lastRead.hasOwnProperty('key')}
					<div id="last-read" class="flex flex-row w-full md:w-max">
						<a href="/{lastReadChapter}/{lastReadVerse}" class="py-2.5 w-full truncate {buttonOutlineClasses} !space-x-0">
							<span class="hidden md:block">Continue Reading: {quranMetaData[lastReadChapter].transliteration}, {lastReadChapter}:{lastReadVerse} {@html '&#10230'}</span>
							<span class="block md:hidden">Continue: {lastReadChapter}:{lastReadVerse} </span>
						</a>
					</div>
				{/if}
			</div>

			<!-- surahs tab -->
			<div class="{cardGridClasses} grid-cols-2">
				{#each chapterListOrder as { id }, i}
					{#if id > 0}
						<a href="/{id}">
							<div class="{cardInnerClasses} flex-col-reverse md:flex-row text-center items-center">
								<div class="">
									<!-- chapter name and icon -->
									<div class="flex flex-row items-center space-x-1 justify-center md:justify-start truncate">
										<div>{id}. {quranMetaData[id].transliteration}</div>
										<div class="opacity-50"><svelte:component this={quranMetaData[id].revelation === 1 ? Mecca : Madinah} /></div>
										<Tooltip arrow={false} type="light" placement="top" class="z-30 hidden md:block font-filter font-normal">{quranMetaData[id].revelation === 1 ? term('meccan') : term('medinan')} revelation</Tooltip>
									</div>

									<!-- chapter translation -->
									<div class="block text-xs opacity-70 truncate">
										{quranMetaData[id].translation}
									</div>

									<!-- chapter verses -->
									<div class="block text-xs opacity-70">
										{quranMetaData[id].verses}
										{term('verses')}
									</div>
								</div>
								<div class="invisible chapter-icons justify-items-end opacity-70 text-5xl">{@html `&#xE9${quranMetaData[id].icon};`}</div>
							</div>
						</a>
					{/if}
				{/each}
			</div>
		</div>

		<!-- suggestions tab -->
		<div class="homepage-tab-panels space-y-12 {activeTab === 2 ? 'block' : 'hidden'}" id="suggestions-tab-panel" role="tabpanel" aria-labelledby="suggestions-tab">
			<div id="suggestions-chapters" class="flex flex-col space-y-4">
				<div class="{cardGridClasses} grid-cols-1">
					{#each Object.entries(mostRead) as [id, item]}
						<a href={item.url} class="!justify-start {cardInnerClasses} flex-col">
							<span class="text-sm">{quranMetaData[item.chapter].transliteration} ({item.verses})</span>
							<div class="block text-xs opacity-70">{item.title}</div>
						</a>
					{/each}
				</div>

				<div class="px-2 text-xs opacity-70">Suggestions listed here are based on the most frequently read chapters and verses by muslim audience, as well as virtues derived from Hadiths. While some Hadiths highlighting these virtues may be considered weak by some scholars, using them for beneficial knowledge is also a widely accepted opinion.</div>
			</div>
		</div>

		<!-- bookmarks tab -->
		<div class="bookmarks-tab-panels space-y-12 {activeTab === 3 ? 'block' : 'hidden'}" id="bookmarks-tab-panel" role="tabpanel" aria-labelledby="bookmarks-tab">
			<div id="bookmark-cards" class="flex flex-col space-y-4">
				{#if totalBookmarks === 0}
					<div class="flex items-center justify-center text-sm opacity-70">You currently do not have any bookmarks.</div>
				{:else}
					<div class="{cardGridClasses} grid-cols-1">
						{#each $__userBookmarks as bookmark}
							<div class="flex flex-row space-x-2">
								<a href="{bookmark.split(':')[0]}/{bookmark.split(':')[1]}" class="!justify-start {cardInnerClasses} w-full flex-col">
									<div class="text-sm">{quranMetaData[bookmark.split(':')[0]].transliteration} ({bookmark})</div>

									{#if activeTab === 3 && totalBookmarks !== 0}
										<div class="text-sm opacity-70 truncate direction-rtl text-right arabic-font-1">
											{#await fetchData then data}
												{data[bookmark].words.arabic.split(splitDelimiter).join(' ')}
											{:catch error}
												<p></p>
											{/await}
										</div>
									{/if}
								</a>

								<!-- delete/cross button -->
								<button on:click={() => updateSettings({ type: 'userBookmarks', key: bookmark })} class="pointer h-7 w-7 opacity-100" style="margin-left: -20px; margin-top: -5px;" title="Remove bookmark"><CrossSolid size={7} /></button>
							</div>
						{/each}
					</div>
				{/if}
			</div>
		</div>

		<!-- notes tab -->
		<div class="notes-tab-panels space-y-12 {activeTab === 4 ? 'block' : 'hidden'}" id="notes-tab-panel" role="tabpanel" aria-labelledby="notes-tab">
			<div id="notes-cards" class="flex flex-col space-y-4">
				{#if totalNotes === 0}
					<div class="flex items-center justify-center text-sm opacity-70">You currently do not have any saved notes.</div>
				{:else}
					<div class="{cardGridClasses} grid-cols-1">
						{#each Object.entries($__userNotes) as [verse, note]}
							<a href="{verse.split(':')[0]}/{verse.split(':')[1]}" class="!justify-start {cardInnerClasses} w-full flex-col">
								<div class="text-sm">{quranMetaData[verse.split(':')[0]].transliteration} ({verse})</div>
								<span class="text-xs opacity-70 truncate">{note.note}</span>
							</a>
						{/each}
					</div>
				{/if}
			</div>
		</div>
	</div>
</div>
