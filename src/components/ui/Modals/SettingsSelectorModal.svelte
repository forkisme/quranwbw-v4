<script>
	import Modal from '$ui/FlowbiteSvelte/modal/Modal.svelte';
	import DisplayTypeSelector from '$ui/SettingsDrawer/DisplayTypeSelector.svelte';
	import { __settingsSelectorModal } from '$utils/stores';
	import { getModalTransition } from '$utils/getModalTransition';
</script>

<Modal id="settingsSelectorModal" bind:open={$__settingsSelectorModal.visible} transitionParams={getModalTransition('bottom')} size="xs" class="!rounded-b-none md:!rounded-3xl theme" bodyClass="p-6" position="bottom" center outsideclose>
	<h3 id="modal-title" class="mb-8 text-xl font-medium text-black">{$__settingsSelectorModal.title}</h3>
	<div class="max-h-[70vh] overflow-y-scroll pr-2"><DisplayTypeSelector /></div>
</Modal>
