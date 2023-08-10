<script setup>
import { defineEmits } from "vue";
import { RouterLink } from "vue-router";

defineEmits(["open-modal"]);
</script>
<template>
	<nav class="navbar">
		<h1 class="logo">Notes</h1>
		<button class="add-btn" @click="$emit('open-modal', true)">+</button>
	</nav>
</template>

<style lang="scss" scoped>
.navbar {
	padding: 1rem 0;
	width: 80%;
	max-width: 1024px;
	display: flex;
	margin: 0 auto;
	align-items: center;
	justify-content: space-between;
	a {
		text-decoration: none;
	}
	.logo {
		font-size: 2.5rem;
		font-weight: bold;
		color: var(--primary-color);
	}
	.add-btn {
		background-color: var(--primary-color);
		height: 40px;
		width: 40px;
		border: none;
		border-radius: 50%;
		color: var(--light-100);
		font-size: 1.75rem;
		text-align: center;
		cursor: pointer;
		transition: opacity 300ms;
		box-shadow: var(--shadow-100);

		&:active {
			opacity: 0.5;
		}
	}
}
</style>
