<script>
	import Header from './components/Header.svelte';
	import Footer from './components/Footer.svelte';
	import Scanner from './components/Scanner.svelte';

	import './app.css';

	let result;
</script>

<Header />

<main>
	<!-- <Scanner active={!result} onSuccessfulScan={(qrCodeData) => result = qrCodeData} let:result > -->
	<Scanner bind:result>
		<!-- override default by placing handler in here  -->
		<!-- <Results active={result !== null} decodedData={result} onNewScan={() => result = null} /> -->
		{#if result}
			<div>
				The result is: {result}
			</div>
			<div>
				<button on:click={() => (result = null)}>Scan again</button>
			</div>
		{/if}
	</Scanner>
</main>

<Footer />
