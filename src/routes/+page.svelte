<script>
	import { Heading, Span, Button } from 'flowbite-svelte';
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Genshin Impact tools" />
</svelte:head>

<div class="text-center">
	<Heading tag="h1" class="mt-20 mb-4" customSize="text-4xl font-extrabold md:text-5xl lg:text-6xl">
		Welcome to <Span gradient>tahubulat.tech</Span>
	</Heading>
	<Heading tag="h6" class="mb-20">
		A Genshin Impact tools made by me because I'm bored lmao.
	</Heading>

	<Heading tag="h2" class="mb-4" customSize="text-4xl font-extrabold">Features</Heading>

	<Button href="/uncensor" gradient color="purpleToBlue" size="lg">Bypass Text Censorship</Button>
	<Button href="/stats" gradient color="cyanToBlue" size="lg">Player Stats</Button>
</div>
