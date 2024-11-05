<script lang="ts">
	import '../app.css';
	import logo from '$lib/assets/logo.png';
	let { children } = $props();
</script>

<header class="flex justify-between px-8 py-4 text-xl">
	<a href="/" class="flex items-center justify-center gap-3 text-lg font-semibold uppercase">
		<img class="h-[50px]" src={logo} alt="Logo VoteApp - voting by placing a ballot into a box" />
		<h1>VoteApp</h1>
	</a>
	<nav class="flex gap-6">
		<a class="font-semibold hover:text-red-800" href="/">Home</a>
		<a class="font-semibold hover:text-red-800" href="/about">About</a>
	</nav>
</header>
<main class="m-auto mt-10 p-4 md:mt-[100px] md:w-3/4 md:p-0">
	{@render children()}
</main>
