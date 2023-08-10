<script setup>
import { RouterLink } from "vue-router";
import { Icon } from "@iconify/vue";
</script>
<template>
	<nav>
		<div class="main-wrapper">
			<RouterLink to="/"
				><div class="logo">
					<Icon icon="arcticons:memo-game" width="30" /></div
			></RouterLink>
			<ul>
				<RouterLink to="/"><li>Home</li></RouterLink>
				<RouterLink to="/about"><li>About</li></RouterLink>
			</ul>
		</div>
	</nav>
</template>

<style lang="scss" scoped>
nav {
	width: 100%;
	padding: 1rem 0;
}
.main-wrapper {
	margin: 0 auto;
	display: flex;
	align-items: center;
	justify-content: space-between;
}
.logo {
	display: flex;
	place-items: center;
	color: black;
}
ul {
	display: flex;
	gap: 2rem;
}
a {
	text-decoration: none;
	color: var(--secondary-color);
	transition: 0.4s;
}
a:hover {
	background-color: var(--bg-100);
}
</style>
