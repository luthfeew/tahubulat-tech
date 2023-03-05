<script>
	import { page } from '$app/stores';
	import { Navbar, NavBrand, NavLi, NavUl, NavHamburger, DarkMode } from 'flowbite-svelte';
</script>

<Navbar let:hidden let:toggle rounded color="form">
	<NavBrand href="/">
		<span class="self-center whitespace-nowrap text-xl font-semibold dark:text-white">
			TahuBulat
		</span>
	</NavBrand>
	<div class="flex md:order-2">
		<DarkMode />
		<NavHamburger on:click={toggle} />
	</div>
	<NavUl {hidden} class="order-1">
		<NavLi href="/" active={$page.url.pathname === '/' ? true : false}>Home</NavLi>
		<NavLi href="/uncensor" active={$page.url.pathname === '/uncensor' ? true : false}>
			Bypass Text Censorship
		</NavLi>
		<NavLi href="/stats" active={$page.url.pathname === '/stats' ? true : false}>
			Player Stats
		</NavLi>
	</NavUl>
</Navbar>
