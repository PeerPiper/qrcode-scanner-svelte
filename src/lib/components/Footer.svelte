<script>
</script>

<footer>
	<span class="footer__text">Source-code available at</span>
	<a
		href="http://github.com/peerpiper/qrcode-scanner-svelte"
		class="footer__project-link"
		target="_blank"
		rel="noopener noreferrer"
	>
		peerpiper/qrcode-scanner-svelte
	</a>
</footer>

<style>
	footer {
		display: flex;
		align-items: center;
		justify-content: center;

		margin-bottom: 20px;
		padding: 20px;

		font-size: 0.8rem;
	}

	.footer__project-link {
		padding: 3px 7px;

		transition: background 200ms;

		text-decoration: none;

		color: white;
		border-radius: 8px;
		background-color: #161616;

		font-weight: 500;
	}

	.footer__project-link:hover {
		background-color: #06bd00;
	}

	.footer__text {
		margin-right: 5px;
	}
</style>
