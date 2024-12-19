<template>
	<Transition name="modal-outer">
		<div
			v-show="modalActive"
			class="w-full h-screen flex bg-opacity-30 bg-black px-8 top-0 left-0 justify-center"
		>
			<Transition name="modal-inner">
				<div
					v-if="modalActive"
					class="p-4 bg-white self-start mt-32 max-w-screen-md"
				>
					<slot />
					<button
						class="mt-8 text-white bg-weather-primary px-6 py-2"
						@click="$emit(close - modal)"
					>
						Close
					</button>
				</div>
			</Transition>
		</div>
	</Transition>
</template>

<script setup>
defineEmits(["close-modal"]);
defineProps({
	modalActive: {
		type: Boolean,
		default: false,
	},
});
</script>

<style scoped>
.modal-outer-enter-active,
.modal-outer-leave-active {
	transition: opacity 0.3s cubic-bezier(0.52, 0.02, 0.19, 1.02);
}
</style>
