<script>
	import Modal from '$ui/FlowbiteSvelte/modal/Modal.svelte';
	import Translations from '$display/verses/translations/Translations.svelte';
	import SingleArabicVerse from '$display/verses/SingleArabicVerse.svelte';
	import { __currentPage, __verseTranslationModalVisible, __chapterData, __verseKey } from '$utils/stores';
	import { quranMetaData } from '$data/quranMeta';

	let chapterData;
	$: chapter = +$__verseKey.split(':')[0];
	$: verse = +$__verseKey.split(':')[1];
	$: if ($__verseTranslationModalVisible) chapterData = $__currentPage === 'mushaf' ? JSON.parse(localStorage.getItem('pageData')) : $__chapterData;
</script>

<Modal
	bind:open={$__verseTranslationModalVisible}
	title="{quranMetaData[chapter].transliteration}, {chapter}:{verse}"
	id="verseTranslationModal"
	class="!rounded-b-none md:!rounded-3xl theme"
	bodyClass="p-6 space-y-4 flex-1 overflow-y-auto overscroll-contain theme-grayscale"
	headerClass="flex justify-between items-center p-6 rounded-t-3xl text-black theme-grayscale"
	classFooter="rounded-b-3xl flex flex-row justify-between"
	size="lg"
	position="bottom"
	center
	outsideclose
>
	<div class="flex flex-col space-y-4">
		<div class="py-4">
			<SingleArabicVerse key={$__verseKey} />
		</div>

		<Translations isBody={false} value={chapterData[$__verseKey]} />
	</div>
</Modal>
