<script setup>
import { ref } from "vue";
import SearchBar from "../components/SearchBar.vue";
import QuizCard from "../components/QuizCard.vue";
import { Icon } from "@iconify/vue";

const quizzes = ref([]);
const updateQuizzesList = (list) => {
	quizzes.value = list.value;
};
</script>

<template>
	<main class="main-wrapper">
		<SearchBar @search-quizzes="updateQuizzesList" />
		<div v-if="quizzes.length > 0" class="quizzes-wrapper">
			<QuizCard v-for="quiz in quizzes" :quiz="quiz" :key="quiz.id" />
		</div>
		<p v-else class="not-found-msg">
			<Icon icon="noto-v1:sad-but-relieved-face" height="20" />
			<span>No quizzes matching your search, try again!</span>
		</p>
	</main>
</template>

<style lang="scss" scoped>
main {
	flex-direction: column;
	gap: 3rem;
}
.quizzes-wrapper {
	margin: 0 auto;
	display: grid;
	gap: 2rem 1.5rem;
}
.not-found-msg {
	display: flex;
	align-items: center;
	justify-content: center;
	gap: 1rem;
	margin: 0 auto;
	margin-top: 24px;
}
@media (min-width: 550px) {
	.quizzes-wrapper {
		grid-template-columns: repeat(2, 1fr);
	}
}
@media (min-width: 700px) {
	.quizzes-wrapper {
		grid-template-columns: repeat(3, 1fr);
	}
}
@media (min-width: 1150px) {
	.quizzes-wrapper {
		grid-template-columns: repeat(4, 1fr);
	}
}
</style>
