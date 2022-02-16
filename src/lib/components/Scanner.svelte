<script lang="ts">
	import { onMount, createEventDispatcher } from 'svelte';
	import { stream, error, status } from '../stores.js';

	import jsQR from 'jsqr';

	import ScannerBorders from './ScannerBorders.svelte';
	import Results from './Results.svelte';

	import UserMedia from '../utils/use-usermedia.svelte';

	export let result = null; // : string
	export let stopMediaStream = null;
	let startMediaStream;

	const dispatch = createEventDispatcher();

	$: active = !result;

	let video: HTMLVideoElement = null;
	let canvas: HTMLCanvasElement = null;
	let useUserMedia;
	let mounted;

	onMount(() => {
		mounted = true;

		({ stopMediaStream, startMediaStream } = useUserMedia());

		return () => {
			console.log('Component destroyed');
			stopMediaStream();
			video.srcObject = null;
		};
	});

	const startCapturing = (): void => {
		if (!canvas || !video) return;

		const context = canvas.getContext('2d');

		if (!context) return;

		const { width, height } = canvas;

		context.drawImage(video, 0, 0, width, height);

		const imageData = context.getImageData(0, 0, width, height);
		const qrCode = jsQR(imageData.data, width, height);

		if (qrCode === null) {
			console.log('timeout');
			setTimeout(startCapturing, 750);
		} else {
			result = qrCode.data;
			dispatch('successfulScan', qrCode.data);

			stopMediaStream();
			video.srcObject = null;
		}
	};

	const handleCanPlay = (): void => {
		console.log('canplay');
		if (canvas === null || canvas === null || video === null || video === null) {
			return;
		}

		canvas.width = video.videoWidth;
		canvas.height = video.videoHeight;

		if ($error !== null) {
			// TODO: show dialog to user with an error
		} else {
			startCapturing();
		}
	};

	$: if ($status === 'resolved' && video !== null && $stream) {
		console.log('Resolve, stream');
		video.srcObject = $stream;
		video.play().catch(console.error);
	}

	$: if (active && $status === 'stopped' && startMediaStream) {
		startMediaStream();
	}
</script>

<UserMedia bind:useUserMedia />

<div class={`scanner ${active ? '' : 'scanner--hidden'}`}>
	<div class="scanner__aspect-ratio-container">
		<canvas bind:this={canvas} class="scanner__canvas" />
		<!-- svelte-ignore a11y-media-has-caption -->
		<video bind:this={video} on:canplay={handleCanPlay} class="scanner__video">
			<!-- <track kind="captions" /> -->
		</video>
		<ScannerBorders />
	</div>

	<div class="scanner-tip">
		<div>Scan a QR code with your camera to see what it says.</div>
	</div>
</div>

<slot {result}>
	<Results active={result !== null} decodedData={result} onNewScan={() => (result = null)} />
</slot>

<style>
	.scanner {
		width: 100%;
		max-width: 500px;
	}

	.scanner--hidden {
		display: none;
	}

	.scanner__aspect-ratio-container {
		position: relative;

		overflow: hidden;

		padding-bottom: 100%;

		border-radius: 10%;
	}

	.scanner__video {
		position: absolute;
		top: 0;
		left: 0;

		width: 100%;
		height: 100%;

		border-radius: inherit;

		outline: none;
		object-fit: cover;
	}

	.scanner__canvas {
		display: none;
	}

	.scanner-tip {
		display: flex;
		justify-content: center;

		margin-top: 15px;

		font-size: 0.8rem;
	}
</style>
