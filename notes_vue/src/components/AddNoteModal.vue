<script setup>
import { ref } from "vue";

defineProps({
	open: Boolean,
	required: true,
});

const emit = defineEmits(["open-modal", "add-note"]);
const note = ref({
	text: "",
	alert: false,
	msg: "",
});

const addText = () => {
	const text = note.value.text;
	note.value.alert = false;

	if (text.length < 10) {
		note.value.alert = true;
		note.value.msg = "Your note must be longer than 10 char!";
		return;
	}
	if (text.length > 250) {
		note.value.alert = true;
		note.value.msg = "Your note can't be longer than 250 char!";
		return;
	}

	note.value.msg = "";
	emit("open-modal", false);
	emit("add-note", text);
	note.value.text = "";
};
</script>
<template>
	<main class="modal-screen" :class="{ show: open }">
		<div class="modal">
			<div class="header-modal">
				<p
					class="alert-msg"
					:class="{
						'show-alert':
							(note.alert && note.text.length < 10) || !note.text.length > 250,
					}"
				>
					{{ note.msg }}
				</p>
				<button
					class="close-btn"
					@click="
						$emit('open-modal', false);
						note.text = '';
						note.msg = '';
					"
				>
					X
				</button>
			</div>
			<textarea
				v-model.trim="note.text"
				name="add-note"
				id="add-note"
				cols="30"
				rows="10"
				placeholder="Add your note here!"
			></textarea>
			<p
				v-show="note.text.length"
				:class="{
					'alert-color': note.text.length < 10 || note.text.length > 250,
				}"
			>
				{{ note.text.length }}/250
			</p>
			<button class="add-btn" @click="addText">Add Note</button>
		</div>
	</main>
</template>

<style lang="scss" scoped>
.modal-screen {
	top: 0;
	left: 0;
	display: flex;
	align-items: center;
	justify-content: center;
	position: absolute;
	width: 100vw;
	height: 100vh;
	background-color: rgba(0, 0, 0, 0.4);
	z-index: 9;
	transform: scale(0);
	transition: all 500ms ease-in-out;

	.modal {
		display: flex;
		flex-direction: column;
		padding: 1rem;
		height: 50%;
		min-height: 250px;
		width: 85%;
		max-width: 500px;
		background-color: var(--light-100);
	}
	.alert-msg {
		color: red;
		opacity: 0;
		transition: color 300ms;
	}
	.header-modal {
		display: flex;
		align-items: center;
		justify-content: space-between;
	}
	textarea {
		padding: 0.5rem;
		font-size: 1rem;
		&:focus-within {
			border-color: var(--secondary-color);
			outline: none;
		}
	}
	.close-btn {
		background-color: transparent;
		font-size: 1rem;
		margin-bottom: 0.5rem;
		font-weight: 700;
		cursor: pointer;
	}
	.add-btn {
		width: 100%;
		padding: 0.5rem 0;
		margin-top: 0.75rem;
		background-color: var(--secondary-color);
		color: var(--light-100);
		font-size: 1rem;
		cursor: pointer;
		transition: opacity 300ms;
		&:active {
			opacity: 0.5;
		}
	}
	.show-alert {
		opacity: 1;
	}
}
.show {
	transform: scale(1);
}
.alert-color {
	color: var(--alert);
}
</style>
