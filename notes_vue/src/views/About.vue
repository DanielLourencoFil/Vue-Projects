<script setup>
import { Icon } from "@iconify/vue";
import { RouterLink } from "vue-router";
</script>
<template>
	<main class="about main-wrapper">
		<h1>About Notes App</h1>
		<p>
			Notes is a CRUD Todo tracking application created using Vue Js by
			<a href="https://daniel-lourenco-portfolio.netlify.app/" target="_blank"
				>Daniel Lourenço.</a
			>
			<br />Register your daily insight's using this app with with abilities to
			save, complete, edit & delete notes.
		</p>
		<RouterLink to="/"
			><div class="home-icon">
				<Icon
					icon="clarity:home-solid"
					color="rgba(68, 68, 68, 0.5)"
					width="60"
				/></div
		></RouterLink>
	</main>
</template>

<style lang="scss" scoped>
.about {
	display: flex;
	flex-direction: column;
	align-items: center;
	height: 100vh;
	padding: 5rem 0;
	gap: 3rem;

	p {
		max-width: 400px;
		line-height: 1.5;
		text-align: justify;
	}
}
.home-icon {
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
</style>
