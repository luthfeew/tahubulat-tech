<script>
	// @ts-nocheck

	import {
		Breadcrumb,
		BreadcrumbItem,
		Card,
		Button,
		Input,
		Spinner,
		Alert,
		Table,
		TableBody,
		TableBodyCell,
		TableBodyRow,
		TableHead,
		TableHeadCell,
		Listgroup,
		Avatar
	} from 'flowbite-svelte';

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
		<Card class="mb-4">
			<h5 class="mb-2 text-2xl font-bold tracking-tight text-gray-900 dark:text-white">Info</h5>
			<p class="font-normal text-gray-700 dark:text-gray-400 leading-tight">
				<Table>
					<TableBody>
						{#each Object.entries(data.data.info) as [key, value]}
							{#if value}
								<TableBodyRow>
									<TableBodyCell tdClass="p-1 whitespace-nowrap font-medium" style="width:60%"
										>{key}</TableBodyCell
									>
									<TableBodyCell tdClass="p-1 whitespace-nowrap font-medium">{value}</TableBodyCell>
								</TableBodyRow>
							{/if}
						{/each}
						<TableBodyRow>
							<TableBodyCell tdClass="p-1 whitespace-nowrap font-medium" style="width:60%"
								>uid</TableBodyCell
							>
							<TableBodyCell tdClass="p-1 whitespace-nowrap font-medium">{data.uid}</TableBodyCell>
						</TableBodyRow>
					</TableBody>
				</Table>
			</p>
		</Card>

		<Card>
			<h5 class="mb-2 text-2xl font-bold tracking-tight text-gray-900 dark:text-white">Stats</h5>
			<p class="font-normal text-gray-700 dark:text-gray-400 leading-tight">
				<Table>
					<TableBody>
						{#each Object.entries(data.data.stats) as [key, value]}
							<TableBodyRow>
								<TableBodyCell tdClass="p-1 whitespace-nowrap font-medium" style="width:60%"
									>{key}</TableBodyCell
								>
								<TableBodyCell tdClass="p-1 whitespace-nowrap font-medium">{value}</TableBodyCell>
							</TableBodyRow>
						{/each}
					</TableBody>
				</Table>
			</p>
		</Card>

		<Card padding="xl" size="md">
			<div class="flex justify-between items-center mb-4">
				<h5 class="text-xl font-bold leading-none text-gray-900 dark:text-white">Characters</h5>
			</div>
			<Listgroup items={data.data.characters} let:item class="border-0 dark:!bg-transparent">
				<div class="flex space-x-4">
					<div class="flex flex-col space-y-4">
						<Avatar src={item.icon} class="flex-shrink-0" rounded />
						<Avatar src={item.weapon.icon} class="flex-shrink-0" rounded />
					</div>
					<div class="flex-1 min-w-0">
						<Table>
							<TableHead>
								<TableHeadCell colspan="2">{item.name}</TableHeadCell>
							</TableHead>
							<TableBody>
								{#each Object.entries(item) as [key, value], i}
									{#if i > 2 && i < 10 && key !== 'icon' && key !== 'collab'}
										<TableBodyRow color="blue">
											<TableBodyCell tdClass="p-1 whitespace-nowrap font-medium" style="width:60%">
												{key}
											</TableBodyCell>
											<TableBodyCell tdClass="p-1 whitespace-nowrap font-medium">
												{value}
											</TableBodyCell>
										</TableBodyRow>
									{/if}
									{#if key === 'weapon'}
										{#each Object.entries(value) as [key, value], i}
											{#if i > 3 && key !== 'description'}
												<TableBodyRow color="green">
													<TableBodyCell
														tdClass="p-1 whitespace-nowrap font-medium"
														style="width:60%"
													>
														{key}
													</TableBodyCell>
													<TableBodyCell tdClass="p-1 whitespace-nowrap font-medium">
														{value}
													</TableBodyCell>
												</TableBodyRow>
											{/if}
										{/each}
									{/if}
								{/each}
							</TableBody>
						</Table>
					</div>
				</div>
			</Listgroup>
		</Card>
	{:else}
		<p>EMPTY</p>
	{/if}
{:catch error}
	<p>NOT FOUND / User's data is not public.</p>
{/await}
