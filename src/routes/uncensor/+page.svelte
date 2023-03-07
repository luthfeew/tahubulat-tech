<script>
	import { basic, extended } from './word';
	import { Breadcrumb, BreadcrumbItem, Card, Button, Input, Toggle } from 'flowbite-svelte';

	let value = '';
	let checked = false;
	let done = false;

	$: disabled = !value;

	function process() {
		if (!value) return;

		let result = '';

		if (checked) {
			result = value.replace(/./g, (c) => extended[c] || c);
		} else {
			result = value.replace(/./g, (c) => basic[c] || c);
		}

		navigator.clipboard.writeText(result);
		done = true;
	}
</script>

<svelte:head>
	<title>Bypass Text Censorship</title>
	<meta name="description" content="Bypass Text Censorship" />
</svelte:head>

<div class="flex flex-col items-center justify-center">
	<Breadcrumb class="my-4">
		<BreadcrumbItem href="/" home>Home</BreadcrumbItem>
		<BreadcrumbItem>Bypass Text Censorship</BreadcrumbItem>
	</Breadcrumb>
</div>

<Card class="mx-auto">
	<form class="flex flex-col space-y-6" on:submit|preventDefault={process}>
		<h3 class="text-xl font-medium text-gray-900 dark:text-white p-0">Bypass Text Censorship</h3>
		<Input
			type="text"
			placeholder="Type here"
			bind:value
			on:keydown={() => (done = false)}
			color={done === true ? 'green' : 'base'}
		/>
		<Toggle size="small" bind:checked on:change={() => (done = false)}>Extended mode</Toggle>
		<Button {disabled} class="w-full" on:click={process} color={done === true ? 'green' : 'blue'}>
			{#if done}
				Copied
			{:else}
				Process & Copy
			{/if}
		</Button>
	</form>
</Card>
