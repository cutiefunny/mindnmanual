<script>
	import '../app.css';
	import favicon from '$lib/assets/favicon.svg';

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
	<title>마인드앤매뉴얼</title>
	<meta name="description" content="행정안전부 및 대한심폐소생협회 지정 응급처치 전문교육기관" />
</svelte:head>

{@render children()}
