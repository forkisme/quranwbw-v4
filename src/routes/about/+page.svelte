<script>
	import PageHead from '$misc/PageHead.svelte';
	import { __currentPage } from '$utils/stores';
	import { linkClasses } from '$data/commonClasses';

	__currentPage.set('about');
</script>

<PageHead title={'About'} />

<div class="theme">
	<div class="my-6 space-y-4 pb-4">
		<div class="flex flex-col space-y-6text-sm">
			<div id="about-start" class="flex flex-col space-y-6 text-center">
				<div class="text-md md:text-xl font-medium opacity-70 italic w-4/5 mx-auto">
					"Indeed, this (book) is a reminder, so he who wills may take to his Lord a way. And you do not will except that Allah wills. Indeed, Allah is ever Knowing and Wise."
					<a href="/76/29-30" class={linkClasses}>(76:29-30)</a>
				</div>

				<i class="text-sm">In the name of Allah and prayers & blessings be upon His messenger, Muhammad (<span style="font-style: initial; font-weight: normal;">ﷺ</span>).</i>
			</div>

			<div id="about-website" class="flex flex-col space-y-4 mt-8 text-sm">
				<span>QuranWBW.com is your companion for reading, listening to, and learning the Holy Quran, word by word. With features like word audios, Tajweed colors, and transliteration, you can delve into the Quran with ease. Additionally, explore multi-language translations, tafsir, and detailed word morphology.</span>

				<span>
					This website is a passion project, not a full-time endeavor. For issues, suggestions, or contributions, please visit our <a class={linkClasses} target="_blank" rel="noreferrer" href="https://github.com/marwan/quranwbw-v4">GitHub repository</a> or contact us at <a class={linkClasses} href="mailto:quranwbw@gmail.com">quranwbw@gmail.com</a>. You can also join our
					<a class={linkClasses} target="_blank" rel="noreferrer" href="https://chat.whatsapp.com/CtrbWUB4GTyDdZWXWujVSl">WhatsApp group</a> for updates.
				</span>
			</div>

			<div id="website-credits" class="flex flex-col space-y-4 mt-6 text-sm">
				<span>QuranWBW.com owes its existence to the contributions of the following remarkable individuals and organizations:</span>

				<ul class="list-disc ml-5 space-y-2">
					<li><a class={linkClasses} target="_blank" rel="noreferrer" href="http://emuslim.com/Quran/Translation_English.asp">Dr. Shehnaz Shaikh, Ms. Kausar Khatri</a> for their English and <a class={linkClasses} target="_blank" rel="noreferrer" href="https://www.farhathashmi.com">Dr. Farhat Hashmi</a> for her Urdu word by word compilation.</li>
					<li><a class={linkClasses} target="_blank" rel="noreferrer" href="https://quran.com">Quran.com</a> for their API.</li>
					<li><a class={linkClasses} target="_blank" rel="noreferrer" href="http://recitequran.com">ReciteQuran.com </a> for their word by word audio files.</li>
					<li><a class={linkClasses} target="_blank" rel="noreferrer" href="http://qurancomplex.gov.sa/">King Fahd Glorious Quran Printing Complex</a> for their Quran text*.</li>
					<li><a class={linkClasses} target="_blank" rel="noreferrer" href="http://tanzil.net/trans/">Tanzil.net</a> for their collection of Quranic resources.</li>
					<li><a class={linkClasses} target="_blank" rel="noreferrer" href="http://everyayah.com">EveryAyah.com</a>, <a class={linkClasses} href="https://qurancentral.com">QuranCentral.com</a> and <a class={linkClasses} href="https://quranicaudio.com">QuranicAudio.com</a> for their collection of Quranic audios.</li>
					<li><a class={linkClasses} target="_blank" rel="noreferrer" href="https://zoopernet.com">Ayman Siddiqui</a> for all the help including providing various website data and fonts & website testing.</li>
					<li><a class={linkClasses} target="_blank" rel="noreferrer" href="https://github.com/cpfair">Collin Fair</a> for his word timestamps for Quranic audios.</li>
					<li><a class={linkClasses} target="_blank" rel="noreferrer" href="https://play.google.com/store/apps/details?id=com.atq.quranemajeedapp.org.islamone&hl=en">Usman Pervez (IslamOne android app)</a> for various data like Urdu WBW and Tafhim-ul-Quran.</li>
					<li><a class={linkClasses} target="_blank" rel="noreferrer" href="https://gtaf.org">Greentech Apps (Al Quran Tafsir & by Word app)</a> for their English, Indonesian, Bangla, Chinese**, Malayalam*** and Divehi words database and corpus data.</li>
					<li><a class={linkClasses} target="_blank" rel="noreferrer" href="https://github.com/kaisdukes">Dr. Kais Dukes</a> for his morphological description of the Quranic words.</li>
					<li><a class={linkClasses} target="_blank" rel="noreferrer" href="https://www.typemybook.com/download/complete-quran-kareem-text/">Ata Rafi</a> for sharing the IndoPak text originally by <a class={linkClasses} target="_blank" rel="noreferrer" href="http://inpage.com/">InPage™</a>.</li>
					<li><a class={linkClasses} target="_blank" rel="noreferrer" href="https://apps.apple.com/us/app/quran-word-by-word/id588198510">Azrul Azwar (Quran WbW iOS app)</a> for the words databases in many different languages.</li>
					<li><a class={linkClasses} target="_blank" rel="noreferrer" href="http://csalt.itu.edu.pk/urdufont/index.html">CSALT</a> for their Mehr Nastaliq Urdu font.</li>
					<li><a class={linkClasses} target="_blank" rel="noreferrer" href="https://www.alhudapk.com/products/hindi-section.html">Al-Huda International</a> for their Hindi word by word PDF files.</li>
					<li><a class={linkClasses} target="_blank" rel="noreferrer" href="https://www.quran411.com/">Quran411.com</a> for their English transliteration orginally by M.A.H. Eliyasee.</li>
					<li><a class={linkClasses} target="_blank" rel="noreferrer" href="https://www.soundvision.com/article/a-thematic-introduction-to-the-surahs-of-the-qur-an">SoundVision.com</a> for their thematic introduction to the surahs of the Quran.</li>
					<li><a class={linkClasses} target="_blank" rel="noreferrer" href="http://www.tamililquran.com/">TamililQuran.com</a> for their Tamil word by word database orginally by <a class={linkClasses} target="_blank" rel="noreferrer" href="https://www.darulhuda.net/">Darul Huda</a>.</li>
					<li><a class={linkClasses} target="_blank" rel="noreferrer" href="https://alquran.cloud/">Al Quran Cloud</a> and <a class={linkClasses} target="_blank" rel="noreferrer" href="https://github.com/fawazahmed0">Fawaz Ahmed</a> for their Quran translations API.</li>
					<li><a class={linkClasses} target="_blank" rel="noreferrer" href="https://drusaman.github.io/">Dr. Usama Nonnenmacher</a> for his excellent work on the French word by word data.</li>
					<li><a class={linkClasses} target="_blank" rel="noreferrer" href="https://github.com/spa5k/tafsir_api">Spark (spa5k)</a> for his Tafsir API.</li>
					<li><a class={linkClasses} target="_blank" rel="noreferrer" href="https://quran-pro.com/">Quran Pro</a> for their pictures of different recitors.</li>
					<li><a class={linkClasses} target="_blank" rel="noreferrer" href="https://www.flaticon.com/uicons/">Flaticon</a> for their great range of interface icons.</li>
					<li><a class={linkClasses} target="_blank" rel="noreferrer" href="https://qul.tarteel.ai/">Quranic Universal Library (QUL)</a> for their Persian words database.</li>
				</ul>

				<span>All data used on this website belongs to their respective owners and authors. Permission must be obtained from them before copying any content.</span>

				<div class="border-b border-black/10"></div>

				<span class="text-xs opacity-70"
					>*The Mushaf fonts are developed by and are a property of <a class={linkClasses} target="_blank" rel="noreferrer" href="https://qurancomplex.gov.sa/">King Fahad Glorious Quran Printing Complex</a> based in Madinah, Saudi Arabia. Tajweed Colors are based on
					<a class={linkClasses} target="_blank" rel="noreferrer" href="https://easyquran.com/ar/">Tajweed Quran (Easy Quran) Mushaf by Dar-Al-Marifa</a> developed by Dr. Eng. Taha Subhi, Damascus - Syria. Tajweed Color Font Features on King Fahad Glorious Quran Printing Complex V4 Mushaf fonts invented and developed by
					<a class={linkClasses} target="_blank" rel="noreferrer" href="https://zoopernet.com">Ayman Siddiqui</a>
					and co-developed by team : Rania, Amena, Naveed Ahmed, Mohammed Zahid, Tooba and Anza for Sadaqa-e-jaria (non-commercial) purposes only. Permitted to develop with prior permissions secured from King Fahad Glorious Quran Printing Complex and Dar-Al-Marifa by <a class={linkClasses} target="_blank" rel="noreferrer" href="https://quran.com">Quran.com</a> (Quran Foundation) team.</span
				>

				<span class="text-xs opacity-70">
					**We express our heartfelt gratitude to everyone who contributed to the Chinese word-by-word Quran translation, particularly Brother Ma Zhiren for his meticulous translation work completed in 2014, Brother Adam Voo from Malaysia and Brother Usama Nonnenmacher for digitalizing the data, and Brothers Max Ma from Canada and Suleyman Abdullah from China for their collaboration. May Allah
					reward them all abundantly in this world and the hereafter. We pray that Allah blesses our Chinese brothers and sisters through the Quran app.
				</span>

				<span class="text-xs opacity-70"
					>***Original sources for the Malayalam translations are
					<a class={linkClasses} target="_blank" rel="noreferrer" href="https://amanithafseer.com">Amani Thafseer</a>
					and
					<a class={linkClasses} target="_blank" rel="noreferrer" href="https://www.lalithasaram.net/">Lalithasaram</a>
					For the Amani Thafseer translation, Brother Usama Nonnenmacher assisted in formatting the data and adjusting word splitting where knowledge of Malayalam was not required. Brother Ameen Abdussalam, whom he collaborated with via the Amani Thafseer team, provided crucial support for cases needing Malayalam expertise. Regarding the Lalithasaram translation, minimal adjustments were necessary
					beyond converting the format from an SQL file to a standard database file.</span
				>
			</div>
		</div>
	</div>
</div>
