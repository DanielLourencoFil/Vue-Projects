<script setup>
import { ref, defineEmits } from "vue";
import { Icon } from "@iconify/vue";

const props = defineProps({
	note: {
		type: Object,
		required: true,
	},
	index: {
		type: Number,
		required: true,
	},
});

const isEdit = ref(null);
const newNoteText = ref("");
const emit = defineEmits(["edit-note", "delete-note"]);

const noteEdition = (action, index) => {
	if (action === "edit") {
		isEdit.value = true;
		return;
	}
	if (action === "done") {
		emit("edit-note", newNoteText.value, index);
		isEdit.value = false;
		return;
	}
	if (action === "delete") {
		emit("delete-note", index);

		isEdit.value = false;
		return;
	}
};
</script>

<template>
	<article
		class="single-note-card"
		:style="{ 'background-color': note.bgColor }"
	>
		<div class="edit-wrapper">
			<Icon
				v-if="isEdit"
				icon="ph:check-circle"
				color="var(--sucess)"
				@click="noteEdition('done', index)"
			/>
			<Icon
				v-if="!isEdit"
				icon="ph:pencil"
				@click="noteEdition('edit', index)"
			/>
			<Icon
				icon="ph:trash"
				color="var(--alert)"
				@click="noteEdition('delete', index)"
			/>
		</div>
		<p v-if="!isEdit" class="note-text">
			{{ note.text }}
		</p>
		<textarea
			v-show="isEdit"
			name="note"
			id="note"
			rows="7"
			@input="newNoteText = $event.target.value"
			>{{ note.text }}</textarea
		>
		<p class="note-date">{{ note.date }}</p>
	</article>
</template>

<style lang="scss" scoped>
.single-note-card {
	position: relative;
	display: flex;
	flex-direction: column;
	justify-content: space-between;
	min-height: 200px;
	border-radius: 5px;
	padding: 1rem 1.5rem;
	padding-top: 2rem;
	word-break: break-all;
	transition: transform 300ms;
	&:hover {
		transform: scale(1.1);
	}
	&:hover .edit-wrapper svg {
		opacity: 1;
	}

	.edit-wrapper {
		position: absolute;
		top: 0;
		right: 0;
		padding: 0.5rem;
		display: flex;
		align-items: center;
		justify-content: center;
		gap: 0.5rem;

		svg {
			opacity: 0;
			cursor: pointer;
			transition: all 300ms;
			&:hover {
				transform: scale(1.2);
			}
		}
	}
	.note-text {
		margin-bottom: 2rem;
		white-space: break-spaces;
		// word-wrap: break-word;
		font-size: 1rem;
		font-weight: 600;
		color: var(--primary-color);
	}
	.note-date {
		position: absolute;
		bottom: 0;
		right: 0;
		padding: 0.5rem;
		font-size: 0.75rem;
		color: var(--primary-color);
		opacity: 0.7;
	}
	textarea {
		padding: 0.25rem;
		background-color: transparent;
		border: none;
		width: 100%;
		height: inherit;
		font-size: 1rem;
		font-weight: 600;
		color: var(--primary-color);
		border: 1px solid #74747485;
		&:focus {
			outline: none;
		}
	}
}
</style>
