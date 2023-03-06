<script>
	import { Heading, Span, Card } from 'flowbite-svelte';
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
</div>

<div class="flex flex-col items-center justify-center mb-4">
	<div class="mb-2">
		<Heading tag="h3">Features</Heading>
	</div>
	<div class="grid gap-2 grid-cols-1 sm:grid-cols-2">
		<div>
			<Card href="/uncensor">
				<h5 class="mb-2 text-xl font-bold tracking-tight text-gray-900 dark:text-white">
					Bypass Text Censorship
				</h5>
				<p class="font-normal text-gray-700 dark:text-gray-400 leading-tight">
					Bypass text censorship in Genshin Impact and any other game maybe?
				</p>
			</Card>
		</div>
		<div>
			<Card href="/stats">
				<h5 class="mb-2 text-xl font-bold tracking-tight text-gray-900 dark:text-white">
					Player Stats
				</h5>
				<p class="font-normal text-gray-700 dark:text-gray-400 leading-tight">
					Fetch player statistics from HoYoLAB using Genshin UID.
				</p>
			</Card>
		</div>
	</div>
</div>
