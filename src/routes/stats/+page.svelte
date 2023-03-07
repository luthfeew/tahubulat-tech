<script>
	import { Breadcrumb, BreadcrumbItem, Card, Button, Input, Spinner, Alert } from 'flowbite-svelte';

	const endpoint = 'https://genshinapi-3-e7518255.deta.app/full/';
	let promise = null;
	let value = '';
	let loading = false;

	$: disabled = value.length < 9;

	function sanitize() {
		value = value.replace(/[^0-9]/g, '').slice(0, 9);
	}

	function process() {
		if (value.length < 9) return;
		promise = getData();
	}

	async function getData() {
		loading = true;
		const res = await fetch(`${endpoint}${value}`);
		const data = await res.json();
		loading = false;

		if (res.ok) {
			// console.log(data);
			return data;
		} else {
			throw new Error(data);
		}
	}
</script>

<svelte:head>
	<title>Player Stats</title>
	<meta name="description" content="Player Stats" />
</svelte:head>

<div class="flex flex-col items-center justify-center">
	<Breadcrumb class="my-4">
		<BreadcrumbItem href="/" home>Home</BreadcrumbItem>
		<BreadcrumbItem>Player Stats</BreadcrumbItem>
	</Breadcrumb>
</div>

<Card class="mx-auto">
	<form class="flex flex-col space-y-6" on:submit|preventDefault={process}>
		<h3 class="text-xl font-medium text-gray-900 dark:text-white p-0">Player Stats</h3>
		<Input id="search" placeholder="Enter UID..." size="lg" bind:value on:input={sanitize}>
			<Button {disabled} slot="right" size="sm" on:click={process}>
				{#if loading}
					<Spinner size="5" />
				{:else}
					Show
				{/if}
			</Button>
		</Input>
	</form>
</Card>

<Alert border color="yellow" class="my-4">
	<span class="font-medium">Warning alert!</span> This feature is still in development. Some data may
	not be available.
</Alert>

{#await promise}
	<p>...waiting</p>
{:then data}
	{#if data}
		{data.uid}
		{data.data.info.nickname} <br />
		{#each data.data.characters as item, i}
			{i + 1}:{item.name} <br />
		{/each}
		{#each Object.entries(data.data.stats) as [key, value]}
			{key}: {value} <br />
		{/each}
	{:else}
		<p>EMPTY</p>
	{/if}
{:catch error}
	<p>NOT FOUND / User's data is not public.</p>
{/await}
