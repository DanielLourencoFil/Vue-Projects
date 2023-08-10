<script setup>
import { ref} from "vue";
import { Icon } from "@iconify/vue";
import q from "../data/quizzes.json";

const emit = defineEmits(["search-quizzes"]);
const isSearching = ref(false);
const searchText = ref("");
const quizzes = ref(q);

emit("search-quizzes", quizzes);

const filterQuizzes = () => {
	quizzes.value = q.filter((quiz) =>
		quiz.name.toLowerCase().includes(searchText.value.toLowerCase())
	);
	emit("search-quizzes", quizzes);
};
</script>

<template>
	<div class="search-wrapper">
		<h1>Quizzes</h1>
		<div class="input-wrapper" :class="{ 'input-searching': isSearching }">
			<input
				v-model.trim="searchText"
				@click="isSearching = true"
				@input="filterQuizzes"
				type="text"
				placeholder="Search..."
			/>

			<Icon
				icon="fe:search"
				:class="{ 'icon-searching': isSearching }"
				width="20px"
			/>
		</div>
	</div>
</template>

<style lang="scss" scoped>
.search-wrapper {
	display: flex;
	width: 100%;
	margin: 3rem 0;
	align-items: center;
	justify-content: start;
	gap: 1rem;
	h1 {
		font-size: 1.5rem;
	}
	.input-wrapper {
		display: flex;
		align-items: center;
		border: 1px solid var(--primary-color);
		border-radius: 4px;
		justify-content: space-between;
		transition: all 1s;
		width: 20%;
		min-width: 100px;

		svg {
			margin: 0 0.25rem;
			transform: scale(0);
			opacity: 1;
			cursor: pointer;
		}
		input {
			border-radius: 4px;
			font-size: 1rem;
			width: 100%;
			padding: 0.35rem;
			border: none;

			&:focus {
				// width: 100%;
				border: none;
				outline: none;
				display: flex;
			}
		}
	}
	.input-searching {
		width: 40%;
		max-width: 250px;
	}
	.icon-searching {
		transform: scale(1) !important;
	}
}
</style>
