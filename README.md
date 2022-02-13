# PWA QR Code Scanner

QR Code scanner in a Svelte Component

## Use

[Demo](https://peerpiper.github.io/qrcode-scanner-svelte)

Basic

```js
<Scanner />
```

Custom

```js

    let result

    <Scanner bind:result>
        <!-- Insert custom results component if you want to do something unique with the QR code data -->
		<!-- override default by placing handler in here  -->
		{#if result}
			<div>
				The result is: {result}
			</div>
			<div>
				<button on:click={() => (result = null)}>Scan again</button>
			</div>
		{/if}
	</Scanner>
```
