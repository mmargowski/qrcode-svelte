<script lang="ts">
	let url = '';
	let showQR = false;
	let generatedUrl = '';

	const generateQr = () => {
		console.log(url);
		generatedUrl = url;
		url = '';
		showQR = true;
	};

	const download = async () => {
		let res = await fetch(
			`https://api.qrserver.com/v1/create-qr-code/?size=150x150&data=${generatedUrl}`
		);
		let blob = await res.blob();
		let a = document.createElement('a');
		a.href = window.URL.createObjectURL(blob);
		a.download = 'qrcode.png';
		document.body.appendChild(a);
		a.click();
		document.body.removeChild(a);
	};
</script>

<main class="container">
	<h1>QR Code Generator</h1>

	<section>
		<form on:submit={generateQr}>
			<div class="grid">
				<input type="url" bind:value={url} placeholder="Enter URL" />
			</div>
			<button type="submit">Generate</button>
		</form>
	</section>
	<div class="grid" />
	{#if showQR}
		<section>
			<div class="grid">
				<div>
					<p>{generatedUrl}</p>
					<img
						src={`https://api.qrserver.com/v1/create-qr-code/?size=150x150&data=${generatedUrl}`}
					/>
				</div>
				<div><button class="secondary margin-top" on:click={download}>Download QRCode</button></div>
			</div>
		</section>
	{/if}
</main>

<style>
	.margin-top {
		margin-top: 1rem;
	}
</style>
