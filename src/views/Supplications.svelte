<!-- for supplications and duas routes -->
<script>
	import PageHead from '$misc/PageHead.svelte';
	import Individual from '$display/verses/modes/Individual.svelte';
	import Spinner from '$svgs/Spinner.svelte';
	import { __currentPage, __fontType, __displayType, __wordTranslation, __wordTransliteration } from '$utils/stores';
	import { fetchVersesData } from '$utils/fetchData';
	import { errorLoadingDataMessage } from '$data/websiteSettings';
	import { term } from '$utils/terminologies';
	import { supplicationsFromQuran } from '$data/quranMeta';

	// Fetch supplication verses whenever necessary
	$: fetchData = fetchVersesData(getSupplicationKeys(), $__fontType, $__wordTranslation, $__wordTransliteration);

	// Function to get a string of all supplications chapter:verses
	function getSupplicationKeys() {
		let keysArray = [];

		for (const [key] of Object.entries(supplicationsFromQuran)) {
			keysArray.push(key);
		}

		return keysArray.toString();
	}

	// Set the current page to 'supplications'
	__currentPage.set('supplications');
</script>

<PageHead title={`Quranic ${term('supplications')}`} />

<div>
	{#await fetchData}
		<Spinner height="screen" margin="-mt-20" />
	{:then chapterData}
		<div>
			{#each Object.entries(chapterData) as [key, value]}
				<Individual {key} {value} />
			{/each}
		</div>
	{:catch error}
		<p>{errorLoadingDataMessage}</p>
	{/await}
</div>
