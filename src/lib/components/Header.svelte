<script>
	// import logo from '../assets/logo.svg';
</script>

<header>
	<!-- <img src={logo} width={64} height={64} class="header__logo" alt="logo" /> -->

	<div class="header__text-container">
		<div class="header__title">QR Scan Browser</div>
		<div class="header__description">QR Code Scanner in the Browser</div>
	</div>
</header>

<style>
	header::before {
		position: absolute;
		top: 0;
		left: 0;

		width: 100%;
		height: 4px;

		content: '';

		background: linear-gradient(90deg, #00ff00, #1a7937 90%);
	}

	header {
		display: flex;
		align-items: center;
		justify-content: center;

		padding: 30px 20px 20px 20px;
	}

	.header__logo {
		margin-right: 20px;
	}

	.header__text-container {
		margin-top: -0.5rem;
	}

	.header__title {
		margin-bottom: 8px;

		font-size: 2rem;
	}

	.header__description {
		color: #959595;

		font-size: 1rem;
	}
</style>
