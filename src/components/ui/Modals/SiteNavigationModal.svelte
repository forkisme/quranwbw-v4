<script>
	import Modal from '$ui/FlowbiteSvelte/modal/Modal.svelte';
	import Settings2 from '$svgs/Settings2.svelte';
	import TajweedRules from '$svgs/TajweedRules.svelte';
	import Supplication from '$svgs/Supplication.svelte';
	import Bookmark from '$svgs/Bookmark.svelte';
	import Search2 from '$svgs/Search2.svelte';
	import Morphology from '$svgs/Morphology.svelte';
	import Puzzle from '$svgs/Puzzle.svelte';
	import About from '$svgs/About.svelte';
	import Changelogs from '$svgs/Changelogs.svelte';
	import LegacySite from '$svgs/LegacySite.svelte';
	import { __siteNavigationModalVisible, __settingsDrawerHidden, __tajweedRulesModalVisible, __tokenModalVisible, __currentPage } from '$utils/stores';
	import { term } from '$utils/terminologies';
	import { getModalTransition } from '$utils/getModalTransition';

	const linkClasses = 'w-full flex flex-row space-x-2 py-4 px-4 text-gray-600 bg-gray-100 hover:bg-gray-200 rounded-xl items-center cursor-pointer theme-grayscale';
	const linkTextClasses = 'text-xs md:text-sm text-left w-[-webkit-fill-available] truncate';

	// hide the modal when page changes
	$: if ($__currentPage) __siteNavigationModalVisible.set(false);
</script>

<Modal id="siteNavigationModal" bind:open={$__siteNavigationModalVisible} transitionParams={getModalTransition('basic')} size="xs" class="rounded-3xl text-black theme" bodyClass="p-6" center outsideclose>
	<div class="flex flex-col space-y-4">
		<!-- modals / popups -->
		<div class="flex flex-col space-y-2">
			<div class="font-semibold">Navigate</div>
			<div class="grid grid-cols-2 md:grid-cols-2 gap-1">
				<!-- Search -->
				<a href="/search" class={linkClasses}>
					<Search2 size={4} />
					<span class={linkTextClasses}>Search</span>
				</a>

				<!-- settings modal -->
				<button
					on:click={() => {
						__siteNavigationModalVisible.set(false);
						__settingsDrawerHidden.set(false);
					}}
					class={linkClasses}
				>
					<Settings2 size={4} />
					<span class={linkTextClasses}>Settings</span>
				</button>

				<!-- Bookmarks -->
				<a href="/bookmarks" class={linkClasses}>
					<Bookmark size={4} />
					<span class={linkTextClasses}>Bookmarks</span>
				</a>

				<!-- tajweed rules modal -->
				<button
					on:click={() => {
						__siteNavigationModalVisible.set(false);
						__tajweedRulesModalVisible.set(true);
					}}
					class={linkClasses}
				>
					<TajweedRules size={4} />
					<span class={linkTextClasses}>{term('tajweed')} Rules</span>
				</button>

				<!-- Supplications -->
				<a href="/{term('supplications').toLowerCase()}" class={linkClasses}>
					<Supplication size={4} />
					<span class={linkTextClasses}>{term('supplications')}</span>
				</a>

				<!-- Morphology -->
				<a href="/morphology/1:1:1" class={linkClasses}>
					<Morphology size={4} />
					<span class={linkTextClasses}>Morphology</span>
				</a>

				<!-- Guess The Word -->
				<a href="/games/guess-the-word" class={linkClasses}>
					<Puzzle size={4} />
					<span class={linkTextClasses}>Word Game</span>
				</a>

				<!-- Changelogs -->
				<a href="/changelogs" class={linkClasses}>
					<Changelogs size={4} />
					<span class={linkTextClasses}>Changelogs</span>
				</a>

				<!-- legacy site link -->
				<a href="https://v3.quranwbw.com/" target="_blank" class={linkClasses}>
					<LegacySite size={4} />
					<span class={linkTextClasses}>Old Website</span>
				</a>

				<!-- About -->
				<a href="/about" class={linkClasses}>
					<About size={4} />
					<span class={linkTextClasses}>About</span>
				</a>
			</div>
		</div>
	</div>
</Modal>
