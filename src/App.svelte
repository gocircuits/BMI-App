<script>
	import Speedometer from "svelte-speedometer";

	let impUnits = true;
	let h = 66;
	let hf = Math.trunc(h / 12);
	let hi = h % 12;
	let hm = h * 2.54;
	let w = 152;
	let wm = w * 0.45359237;
	let bmi = +((703 * w) / (h * h)).toFixed(1);

	function setFromHf(value) {
		hf = +value;
		h = hf * 12 + hi;
		bmi = +((703 * w) / (h * h)).toFixed(1);
	}
	function setFromHi(value) {
		hi = +value;
		if (hi > 11) {
			hi = hi - 12;
			hf++;
		}
		if (hi < 0) {
			hi = hi + 12;
			hf--;
		}
		h = hf * 12 + hi;
		bmi = +((703 * w) / (h * h)).toFixed(1);
	}
	function setFromHm(value) {
		hm = +value;
		h = hm / 2.54;
		bmi = +((703 * w) / (h * h)).toFixed(1);
	}
	function setFromW(value) {
		w = +value;
		bmi = +((703 * w) / (h * h)).toFixed(1);
	}
	function setFromWm(value) {
		wm = +value;
		w = wm / 0.45359237;
		bmi = +((703 * w) / (h * h)).toFixed(1);
	}
	function setFromB(value) {
		bmi = +value;
		w = +((bmi * h * h) / 703).toFixed(0);
	}
	function toggle() {
		impUnits = !impUnits;
		if (!impUnits) {
			hm = +(h * 2.54).toFixed(0);
			wm = +(w * 0.45359237).toFixed(0);
		} else {
			h = hm / 2.54;
			hi = +(h % 12).toFixed(0);
			hf = Math.trunc(h / 12);
			w = +w.toFixed(0);
		}
	}
</script>

<style>
	button {
		width: 5em;
		display: inline-block;
	}
	input {
		width: 5em;
		display: inline-block;
	}
	input.imp {
		width: 4em;
		display: inline-block;
	}
	.range {
		display: none;
	}
	.blue {
		font-size: 20px;
		color: #00f;
	}
	.glow {
		font-size: 20px;
		color: #f00;
		text-align: center;
		-webkit-animation: glow 1s ease-in-out infinite alternate;
		-moz-animation: glow 1s ease-in-out infinite alternate;
		animation: glow 1s ease-in-out infinite alternate;
	}
	@-webkit-keyframes glow {
		from {
			text-shadow: 0 0 10px #e98d8d, 0 0 20px #f00, 0 0 30px #e60073,
				0 0 40px #e60073, 0 0 50px #e60073, 0 0 60px #e60073,
				0 0 70px #e60073;
		}
		to {
			text-shadow: 0 0 20px #fff, 0 0 30px #ff4da6, 0 0 40px #ff4da6,
				0 0 50px #ff4da6, 0 0 60px #ff4da6, 0 0 70px #ff4da6,
				0 0 80px #ff4da6;
		}
	}
</style>
<h1>Bady Mass Calculator</h1>
<h2>Units</h2>
<button on:click={toggle}> {impUnits ? 'Imperial' : 'Metric'} </button>
<h2>Values</h2>
{#if impUnits}
	height
	<input
		class="imp"
		value={hf}
		on:input={(e) => setFromHf(e.target.value)}
		type="number" />
	ft
	<input
		class="imp"
		value={hi}
		on:input={(e) => setFromHi(e.target.value)}
		type="number" />
	in
{:else}
	height
	<input
		value={hm}
		on:input={(e) => setFromHm(e.target.value)}
		type="number" />
	cm
{/if}
<br />
{#if impUnits}
	weight
	<input value={w} on:input={(e) => setFromW(e.target.value)} type="number" />
	{impUnits ? 'lbs' : 'kg'}
{:else}
	weight
	<input
		value={wm}
		step="0.1"
		on:input={(e) => setFromWm(e.target.value)}
		type="number" />
	{impUnits ? 'lbs' : 'kg'}
{/if}

<br />
bmi
<input
	class:glow={bmi > 30}
	class:blue={bmi < 19}
	step="0.1"
	value={bmi}
	on:input={(e) => setFromB(e.target.value)}
	type="number" />
<br />
<span class:range={bmi > 30 || bmi < 19}>
	<Speedometer
		maxValue={30}
		minValue={19}
		value={bmi}
		needleColor="violet"
		needleHeightRatio={0.7}
		endColor="firebrick"
		startColor="lightgreen"
		segments={12} />
</span>
