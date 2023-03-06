<script>
	import '../app.css';
	import Header from './Header.svelte';
	import Footer from './Footer.svelte';
</script>

<div class="app min-h-screen flex flex-col">
	<Header />

	<main class="container mx-auto px-4 flex-1">
		<slot />
	</main>

	<Footer />
</div>
