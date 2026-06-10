<script>
	import '../app.css';
	import favicon from '$lib/assets/favicon.svg';
	import Header from '$lib/components/Header.svelte';
	import Footer from '$lib/components/Footer.svelte';
	import FloatingActions from '$lib/components/FloatingActions.svelte';

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
	<title>마인드앤매뉴얼</title>
	<meta name="description" content="행정안전부 및 대한심폐소생협회 지정 응급처치 전문교육기관" />
</svelte:head>

<Header />
{@render children()}
<Footer />
<FloatingActions />
