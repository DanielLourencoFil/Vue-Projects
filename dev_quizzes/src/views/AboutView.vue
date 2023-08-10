<template>
	<main class="about main-wrapper">
		<h1>About Quizzes App</h1>
		<p>
			Quizzes is an application created using Vue 3 Js by
			<a href="https://daniel-lourenco-portfolio.netlify.app/" target="_blank"
				>Daniel Lourenço.</a
			>
		</p>
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
		text-align: center;
	}
}
</style>
