<script>
	import '../app.css';
	import '../custom-theme.css';
	import Navbar from './Navbar.svelte';

	let { children } = $props();
</script>
<Navbar/>
{@render children()}