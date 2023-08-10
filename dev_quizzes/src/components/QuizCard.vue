<script setup>
import { defineProps } from "vue";

const props = defineProps({
	quiz: {
		type: Object,
		required: true,
	},
});
</script>
<template>
	<router-link :to="`/quiz/${quiz.id}`">
		<div class="card">
			<img :src="quiz.img" :alt="quiz.name" />
			<div class="info-wrapper">
				<h2>{{ quiz.name }}</h2>
				<p>{{ quiz.questions.length }}/questions</p>
			</div>
		</div>
	</router-link>
</template>

<style lang="scss" scoped>
.card {
	overflow: hidden;
	border-radius: 2%;
	box-shadow: 1px 1px 10px rgba(0, 0, 0, 0.2);
	cursor: pointer;
	transition: transform 0.3s;
	img {
		object-fit: cover;
		width: 100%;

		height: 190px;
	}
	&:hover {
		transform: scale(1.1);
	}
	&:active {
		opacity: 0.7;
	}
	.info-wrapper {
		padding: 0.5rem;
	}
}
</style>
