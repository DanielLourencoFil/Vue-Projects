<script setup>
import AddNoteModal from "../components/AddNoteModal.vue";
import HeaderNotes from "../components/HeaderNotes.vue";
import SingleNoteCard from "../components/SingleNoteCard.vue";

import { RouterLink } from "vue-router";
import { Icon } from "@iconify/vue";
import { uid } from "uid";
import { ref, watch } from "vue";

const openModal = ref(null);
const notes = ref([]);

// Fetch notes from localStorage
const fetchNotes = JSON.parse(localStorage.getItem("notes"));
if (fetchNotes) {
	notes.value = fetchNotes;
}
// updates localStorage every time notes is updated
watch(
	notes,
	() => {
		localStorage.setItem("notes", JSON.stringify(notes.value));
	},
	{ deep: true }
);
const notesCreate = (text) => {
	const randomColorGenerator = () => {
		return `hsl(${Math.floor(Math.random() * 361)}, 100%, 75%)`;
	};

	notes.value.push({
		id: uid(),
		text,
		date: new Date().toLocaleDateString(),
		bgColor: randomColorGenerator(),
	});
};
const editNotes = (text, index) => {
	notes.value[index].text = text;
};
const deleteNotes = (index) => {
	notes.value = notes.value.filter((note, pos) => pos !== index);
};
</script>

<template>
	<AddNoteModal
		:open="openModal"
		@open-modal="(e) => (openModal = e)"
		@add-note="notesCreate"
	/>
	<main class="main-wrapper">
		<HeaderNotes @open-modal="(e) => (openModal = e)" />
		<h1 class="message" v-if="notes.length < 1">
			<Icon icon="noto:grinning-face" color="rgba(68, 68, 68, 0.9)" /><span
				>Please, add your first note here!
			</span>
		</h1>
		<div v-else class="notes-wrapper">
			<SingleNoteCard
				v-for="(note, index) in notes"
				:key="note.id"
				:note="note"
				:index="index"
				@edit-note="editNotes"
				@delete-note="deleteNotes"
			/>
		</div>

		<RouterLink to="/about"
			><div class="about-icon">
				<Icon
					icon="ph:question-fill"
					color="rgba(68, 68, 68, 0.5)"
					width="60"
				/></div
		></RouterLink>
	</main>
</template>

<style lang="scss" scoped>
.about-icon {
	svg {
		position: absolute;
		bottom: 25px;
		right: 0;
		cursor: pointer;
		transition: transform 300ms;
		&:hover {
			transform: scale(1.2);
		}
	}
}
.message {
	display: flex;
	align-items: center;
	justify-content: center;
	text-align: center;
	margin-top: 5rem;
	span {
		margin-left: 0.5rem;
		font-size: 1.5rem;
	}
}
.notes-wrapper {
	margin: 0 auto;
	margin-top: 4rem;
	width: 90%;
	max-width: 250px;
	display: grid;
	grid-template-columns: 1fr;
	gap: 1rem;

	@media screen and (min-width: 500px) {
		grid-template-columns: repeat(2, 1fr);
		max-width: 90%;
	}
	@media screen and (min-width: 700px) {
		grid-template-columns: repeat(3, 1fr);
	}
	@media screen and (min-width: 900px) {
		grid-template-columns: repeat(4, 1fr);
	}
	@media screen and (min-width: 1300px) {
		grid-template-columns: repeat(5, 1fr);
	}
}
</style>
