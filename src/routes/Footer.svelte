<script>
	import { Footer, FooterLinkGroup, FooterLink, FooterCopyright } from 'flowbite-svelte';
</script>

<Footer
	footerType="custom"
	class="p-4 bg-gray-50 rounded-lg shadow md:flex md:items-center md:justify-between md:p-6 dark:bg-gray-700"
>
	<FooterCopyright href="/" by="TahuBulat" />
	<FooterLinkGroup
		ulClass="flex flex-wrap items-center mt-3 text-sm text-gray-500 dark:text-gray-400 sm:mt-0"
	>
		<FooterLink href="/">About</FooterLink>
		<FooterLink href="/">Privacy Policy</FooterLink>
		<FooterLink href="/">Licensing</FooterLink>
		<FooterLink href="/">Contact</FooterLink>
	</FooterLinkGroup>
</Footer>
