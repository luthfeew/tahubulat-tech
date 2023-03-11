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
		Avatar,
		Heading,
		Img,
		AccordionItem,
		Accordion,
		Rating,
		P,
		Hr
	} from 'flowbite-svelte';

	const endpoint = 'https://genshinapi-3-e7518255.deta.app/full/';
	// const endpoint = 'http://127.0.0.1:8000/full/';
	let promise = null;
	let value = '';
	let loading = false;

	// let promise = getData();

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
			let output = {
				uid: data.uid,
				info: data.data.info,
				stats: data.data.stats,
				characters: data.data.characters,
				explorations: data.data.explorations,
				teapot: data.data.teapot,
				abyss: data.data.abyss.current.floors ? data.data.abyss.current : data.data.abyss.previous,
				activities: data.data.activities
			};
			return output;
		} else {
			throw new Error(data);
		}
	}

	function toTitleCase(str) {
		return str.replace(/_/g, ' ').replace(/\w\S*/g, function (txt) {
			return txt.charAt(0).toUpperCase() + txt.substr(1).toLowerCase();
		});
	}

	let tdClass = 'px-2 py-1 whitespace-nowrap font-medium';
	let pClass1 = 'text-sm font-medium text-gray-900 truncate dark:text-white';
	let pClass2 = 'text-sm text-gray-500 truncate dark:text-gray-400';
	let pClass3 = 'text-sm font-medium text-gray-900 dark:text-white';
</script>

<svelte:head>
	<title>GenshinTools - Player Stats</title>
	<meta name="title" content="Genshin Impact Tools - Player Stats" />
	<meta name="description" content="Fetch player statistics from HoYoLAB using Genshin UID." />
	<meta
		name="keywords"
		content="genshin, genshin impact, genshin impact tools, player statistics, player information"
	/>
	<meta name="robots" content="index, follow" />
	<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
	<meta name="language" content="English" />
</svelte:head>

<div class="flex flex-col items-center justify-center">
	<Breadcrumb class="my-4">
		<BreadcrumbItem href="/" home>Home</BreadcrumbItem>
		<BreadcrumbItem>Player Stats</BreadcrumbItem>
	</Breadcrumb>
</div>

<Card class="mx-auto">
	<form class="flex flex-col space-y-4" on:submit|preventDefault={process}>
		<Heading tag="h4">Player Stats</Heading>
		<P>Fetch player statistics from HoYoLAB using Genshin UID.</P>
		<Input
			type="text"
			inputmode="numeric"
			id="search"
			placeholder="Enter UID..."
			size="lg"
			bind:value
			on:input={sanitize}
		>
			<Button {disabled} slot="right" size="sm" on:click={process}>
				{#if loading}
					<Spinner color="white" size="5" />
				{:else}
					Search
				{/if}
			</Button>
		</Input>
	</form>
</Card>

{#await promise then data}
	{#if data}
		<Hr class="my-8" height="h-px" />
		<div class="grid gap-4 grid-cols-1 md:grid-cols-6 xl:grid-cols-9 content-center">
			<!-- Information -->
			<div class="md:col-span-3">
				<Card class="min-w-full">
					<Heading tag="h5">Information</Heading>
					<Table striped={true}>
						<TableBody tableBodyClass="divide-y">
							{#each Object.entries(data.info) as [key, value], i}
								{#if value && i > 0}
									<TableBodyRow>
										<TableBodyCell {tdClass}>{toTitleCase(key)}</TableBodyCell>
										<TableBodyCell {tdClass}>{value}</TableBodyCell>
									</TableBodyRow>
								{/if}
							{/each}
						</TableBody>
					</Table>
				</Card>
			</div>

			<!-- Statistics -->
			<div class="md:col-span-3 md:row-span-2">
				<Card class="min-w-full">
					<Heading tag="h5">Statistics</Heading>
					<Table striped={true}>
						<TableBody tableBodyClass="divide-y">
							{#each Object.entries(data.stats) as [key, value], i}
								{#if value && i > 0}
									<TableBodyRow>
										<TableBodyCell {tdClass}>{toTitleCase(key)}</TableBodyCell>
										<TableBodyCell {tdClass}>{value}</TableBodyCell>
									</TableBodyRow>
								{/if}
							{/each}
						</TableBody>
					</Table>
				</Card>
			</div>

			<!-- Spiral Abyss -->
			<div class="md:col-span-3 md:row-span-2">
				{#if data.abyss}
					<Card class="min-w-full">
						<Heading tag="h5">Spiral Abyss</Heading>
						<Table striped={true}>
							<TableBody tableBodyClass="divide-y">
								{#each Object.entries(data.abyss) as [key, value], i}
									{#if i < 9}
										<TableBodyRow>
											<TableBodyCell {tdClass}>{toTitleCase(key)}</TableBodyCell>
											<TableBodyCell {tdClass}>{value}</TableBodyCell>
										</TableBodyRow>
									{/if}
								{/each}
							</TableBody>
						</Table>
						<Table striped={true}>
							<TableHead>
								<TableHeadCell colspan="4">
									<p class="text-center">Floor</p>
								</TableHeadCell>
							</TableHead>
							<TableBody tableBodyClass="divide-y">
								<TableBodyRow>
									{#each data.abyss.floors as floor}
										<TableBodyCell {tdClass}>
											<p class="text-center">{floor.floor}</p>
										</TableBodyCell>
									{/each}
								</TableBodyRow>
								<TableBodyRow>
									{#each data.abyss.floors as floor}
										<TableBodyCell {tdClass}>
											<Rating count rating={floor.stars} />
										</TableBodyCell>
									{/each}
								</TableBodyRow>
							</TableBody>
						</Table>
						<Accordion flush>
							<AccordionItem>
								<span slot="header" class="pl-2">Detailed Info</span>
								<Heading tag="h6" class="ml-4">Most Played</Heading>
								<Listgroup
									items={data.abyss.ranks.most_played}
									let:item
									class="border-0 dark:!bg-transparent"
								>
									<div class="flex items-center space-x-4">
										<Avatar src={item.icon} class="flex-shrink-0" />
										<div class="flex-1 min-w-0">
											<p class={pClass1}>{item.name}</p>
											<p class={pClass2}>{item.value} times</p>
										</div>
									</div>
								</Listgroup>
								<Heading tag="h6" class="ml-4">Most Kills</Heading>
								<Listgroup
									items={data.abyss.ranks.most_kills}
									let:item
									class="border-0 dark:!bg-transparent"
								>
									<div class="flex items-center space-x-4">
										<Avatar src={item.icon} class="flex-shrink-0" />
										<div class="flex-1 min-w-0">
											<p class={pClass1}>{item.name}</p>
											<p class={pClass2}>{item.value} times</p>
										</div>
									</div>
								</Listgroup>
								<Heading tag="h6" class="ml-4">Strongest Strike</Heading>
								<Listgroup
									items={data.abyss.ranks.strongest_strike}
									let:item
									class="border-0 dark:!bg-transparent"
								>
									<div class="flex items-center space-x-4">
										<Avatar src={item.icon} class="flex-shrink-0" />
										<div class="flex-1 min-w-0">
											<p class={pClass1}>{item.name}</p>
											<p class={pClass2}>{item.value}</p>
										</div>
									</div>
								</Listgroup>
								<Heading tag="h6" class="ml-4">Most Damage Taken</Heading>
								<Listgroup
									items={data.abyss.ranks.most_damage_taken}
									let:item
									class="border-0 dark:!bg-transparent"
								>
									<div class="flex items-center space-x-4">
										<Avatar src={item.icon} class="flex-shrink-0" />
										<div class="flex-1 min-w-0">
											<p class={pClass1}>{item.name}</p>
											<p class={pClass2}>{item.value}</p>
										</div>
									</div>
								</Listgroup>
								<Heading tag="h6" class="ml-4">Most Bursts Used</Heading>
								<Listgroup
									items={data.abyss.ranks.most_bursts_used}
									let:item
									class="border-0 dark:!bg-transparent"
								>
									<div class="flex items-center space-x-4">
										<Avatar src={item.icon} class="flex-shrink-0" />
										<div class="flex-1 min-w-0">
											<p class={pClass1}>{item.name}</p>
											<p class={pClass2}>{item.value} times</p>
										</div>
									</div>
								</Listgroup>
								<Heading tag="h6" class="ml-4">Most Skills Used</Heading>
								<Listgroup
									items={data.abyss.ranks.most_skills_used}
									let:item
									class="border-0 dark:!bg-transparent"
								>
									<div class="flex items-center space-x-4">
										<Avatar src={item.icon} class="flex-shrink-0" />
										<div class="flex-1 min-w-0">
											<p class={pClass1}>{item.name}</p>
											<p class={pClass2}>{item.value} times</p>
										</div>
									</div>
								</Listgroup>
							</AccordionItem>
						</Accordion>
					</Card>
				{/if}
			</div>

			<!-- Teapot -->
			<div class="md:col-span-3">
				{#if data.teapot}
					<Card class="min-w-full">
						<Heading tag="h5">Teapot</Heading>
						<Table striped={true}>
							<TableBody tableBodyClass="divide-y">
								<TableBodyRow>
									<TableBodyCell {tdClass}>Level</TableBodyCell>
									<TableBodyCell {tdClass}>{data.teapot.level}</TableBodyCell>
								</TableBodyRow>
								<TableBodyRow>
									<TableBodyCell {tdClass}>Comfort</TableBodyCell>
									<TableBodyCell {tdClass}>
										{data.teapot.comfort_name} ({data.teapot.comfort})
									</TableBodyCell>
								</TableBodyRow>
								<TableBodyRow>
									<TableBodyCell {tdClass}>Items</TableBodyCell>
									<TableBodyCell {tdClass}>{data.teapot.items}</TableBodyCell>
								</TableBodyRow>
								<TableBodyRow>
									<TableBodyCell {tdClass}>Visitors</TableBodyCell>
									<TableBodyCell {tdClass}>{data.teapot.visitors}</TableBodyCell>
								</TableBodyRow>
							</TableBody>
						</Table>
					</Card>
				{/if}
			</div>

			<!-- Explorations -->
			<div class="col-span-1 md:col-span-6 xl:col-span-9">
				<Card class="min-w-full">
					<Heading tag="h5">Explorations</Heading>
					<Table striped={true}>
						<TableHead>
							{#each data.explorations as exploration}
								<TableHeadCell>
									<p class="text-center">{exploration.name}</p>
								</TableHeadCell>
							{/each}
						</TableHead>
						<TableBody tableBodyClass="divide-y">
							<TableBodyRow>
								{#each data.explorations as exploration}
									<TableBodyCell {tdClass}>
										<Img
											src={exploration.icon}
											size="max-w-xs"
											alignment="mx-auto"
											effect="invert dark:invert-0"
										/>
									</TableBodyCell>
								{/each}
							</TableBodyRow>
							<TableBodyRow>
								{#each data.explorations as exploration}
									<TableBodyCell {tdClass}>
										<Table striped={true}>
											<TableBodyRow>
												<TableBodyCell {tdClass}>Explored</TableBodyCell>
												<TableBodyCell {tdClass}>{exploration.explored}%</TableBodyCell>
											</TableBodyRow>
											<TableBodyRow>
												<TableBodyCell {tdClass}>{exploration.type}</TableBodyCell>
												<TableBodyCell {tdClass}>{exploration.level}</TableBodyCell>
											</TableBodyRow>
										</Table>
									</TableBodyCell>
								{/each}
							</TableBodyRow>
						</TableBody>
					</Table>
				</Card>
			</div>

			<!-- Characters -->
			<div class="col-span-1 md:col-span-4 md:col-start-2 xl:col-span-5 xl:col-start-3">
				<Card class="mb-4 min-w-full">
					<Heading tag="h5" class="ml-4">Characters</Heading>
					<Listgroup items={data.characters} let:item class="border-0 dark:!bg-transparent">
						<Accordion>
							<AccordionItem>
								<div slot="header" class="flex items-center space-x-4">
									<Avatar src={item.icon} class="flex-shrink-0" />
									<Avatar src={item.weapon.icon} class="flex-shrink-0" rounded />
									<div class="flex-1 min-w-0">
										<p class={pClass3}>{item.name}</p>
									</div>
								</div>
								<Table striped={true}>
									<TableHead>
										<TableHeadCell>
											<p class="text-center">Character</p>
										</TableHeadCell>
										<TableHeadCell>
											<p class="text-center">Weapon</p>
										</TableHeadCell>
									</TableHead>
									<TableBody tableBodyClass="divide-y">
										<TableBodyCell tdClass="py-1 whitespace-nowrap font-medium">
											<Table striped={true}>
												{#each Object.entries(item) as [key, value], i}
													{#if i > 1 && i < 10 && key !== 'icon' && key !== 'collab'}
														<TableBodyRow>
															<TableBodyCell {tdClass}>{toTitleCase(key)}</TableBodyCell>
															<TableBodyCell {tdClass}>{value}</TableBodyCell>
														</TableBodyRow>
													{/if}
													{#if key === 'artifacts'}
														<TableBodyRow>
															<TableBodyCell {tdClass}>Artifacts</TableBodyCell>
															<TableBodyCell {tdClass}>
																{#each value as artifact}
																	{artifact.set.name}
																	<br />
																{/each}
															</TableBodyCell>
														</TableBodyRow>
													{/if}
													{#if key === 'outfits' && value.length > 0}
														<TableBodyRow>
															<TableBodyCell {tdClass}>Outfits</TableBodyCell>
															<TableBodyCell {tdClass}>{value[0].name}</TableBodyCell>
														</TableBodyRow>
													{/if}
												{/each}
											</Table>
										</TableBodyCell>
										<TableBodyCell {tdClass}>
											<Table striped={true}>
												{#each Object.entries(item.weapon) as [key, value], i}
													{#if i > 2 && key !== 'icon' && key !== 'description'}
														<TableBodyRow>
															<TableBodyCell {tdClass}>{toTitleCase(key)}</TableBodyCell>
															<TableBodyCell {tdClass}>{value}</TableBodyCell>
														</TableBodyRow>
													{/if}
												{/each}
											</Table>
										</TableBodyCell>
									</TableBody>
								</Table>
							</AccordionItem>
						</Accordion>
					</Listgroup>
				</Card>
			</div>
		</div>
	{/if}
{:catch error}
	<div class="flex items-center justify-center my-4">
		<Alert class="max-w-sm" border color="yellow">
			<span class="font-medium">NOT FOUND / User's data is not public.</span>
			Additional info: {JSON.stringify(error)}
		</Alert>
	</div>
{/await}
