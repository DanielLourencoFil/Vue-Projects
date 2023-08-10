<script setup>
import { onMounted, ref, watch } from "vue";
import q from "../data/quizzes.json";
import { useRoute, RouterLink } from "vue-router";

const route = useRoute();
const quiz = ref({});
const isLoading = ref(true);

const answers = ref({
	number: 0,
	right: 0,
	finish: false,
});
const currentQuestion = ref(0);

const checkAnswer = (question) => {
	if (currentQuestion.value < quiz.value.questions.length - 1) {
		currentQuestion.value++;
	}
	if (answers.value.number < quiz.value.questions.length) {
		answers.value.number++;
		if (question.isCorrect) {
			answers.value.right++;
		}
	}
};
watch(
	answers,
	() => {
		if (answers.value.number >= quiz.value.questions.length) {
			answers.value.finish = true;
		}
	},
	{ deep: true }
);
onMounted(() => {
	const fetchQuiz = q.filter((item) => item.id.toString() === route.params.id);
	quiz.value = fetchQuiz[0];
	isLoading.value = false;
});
</script>
<template>
	<p v-if="isLoading">Loading...</p>
	<main v-else class="main-wrapper quiz-wrapper">
		<section class="status-wrapper">
			<h2>{{ `Questions ${answers.number}/${quiz.questions.length}` }}</h2>
			<div class="progress-bar">
				<div
					class="bar-percentage"
					:style="{
						width: `${(answers.number / quiz.questions.length) * 100}%`,
					}"
				></div>
			</div>
		</section>
		<section class="answers-wrapper">
			<div class="quiz-wrapper" :class="{ 'animation-1': answers.finish }">
				<h1>{{ quiz.questions[currentQuestion].text }}?</h1>
				<div
					v-for="option in quiz.questions[currentQuestion].options"
					class="options"
					@click="checkAnswer(option)"
				>
					<p>{{ option.label }}</p>
					<p>{{ option.text }}</p>
				</div>
			</div>
			<div class="result-wrapper" :class="{ 'animation-2': answers.finish }">
				<p>Your Results...</p>
				<h2>
					{{ `${answers.right}/${answers.number}` }}
				</h2>
				<RouterLink to="/">Go Back</RouterLink>
			</div>
		</section>
	</main>
</template>

<style lang="scss" scoped>
.quiz-wrapper {
	margin-top: 2rem;
	flex-direction: column;
	gap: 2.5rem;
	.status-wrapper {
		display: flex;
		flex-direction: column;
		gap: 0.5rem;
		h2 {
			font-size: 1.15rem;
		}
		.progress-bar {
			width: 50%;
			height: 30px;
			border: 1px solid var(--primary-color);
			.bar-percentage {
				height: 100%;
				width: 0;
				background-color: var(--secondary-color);
				transition: all 0.5s;
			}
		}
	}
	.answers-wrapper {
		position: relative;

		.quiz-wrapper {
			display: flex;
			gap: 0.5rem;

			h1 {
				font-size: 1.5rem;
				margin-bottom: 0.75rem;
			}
			.options {
				display: flex;
				cursor: pointer;
				transition: transform 0.3s;
				will-change: contents;
				width: 90%;
				&:hover {
					transform: scale(1.02);
				}
				p:first-child {
					text-align: center;
					width: 25px;
					background-color: var(--secondary-color);
				}
				p:last-child {
					background-color: #e0e0e0;
					padding-left: 0.5rem;
					width: 80%;
				}
			}
		}

		.result-wrapper {
			position: absolute;
			top: 50%;
			left: 50%;
			display: flex;
			transform: translate(-50%, -50%) scale(0);
			align-items: center;
			justify-content: center;
			flex-direction: column;
			gap: 1rem;

			p {
				font-size: 1.5rem;
			}
			h2 {
				font-size: 3rem;
			}
			a {
				font-size: 0.75rem;
				padding: 0.25rem 1rem;
				background-color: var(--secondary-color);
				text-align: center;
				border-radius: 0.5rem;
				color: var(--primary-color);
				border: 1px solid transparent;
				&:hover {
					opacity: 0.8;
					border-color: var(--primary-color);
				}
				&:active {
					opacity: 0.7;
				}
			}
		}
		.animation-1 {
			animation: finish-animation-1 1.25s 0.5s ease-in-out forwards;
		}
		.animation-2 {
			animation: finish-animation-2 1.25s 1.75s ease-in-out forwards;
		}
		@keyframes finish-animation-1 {
			0% {
				transform: scale(1);
			}
			25% {
				transform: scale(1.2);
			}

			100% {
				transform: scale(0);
			}
		}
		@keyframes finish-animation-2 {
			0% {
				transform: translate(-50%, -50%) scale(0);
			}

			75% {
				transform: translate(-50%, -50%) scale(1.2);
			}
			100% {
				// transform: scale(1);
				transform: translate(-50%, -50%) scale(1);
			}
		}
	}
}
</style>
