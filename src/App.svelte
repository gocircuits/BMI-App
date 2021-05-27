<script>
	import {Row,Col} from 'svelte-chota';
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
<Row>
	<Col> <h1 class="is-center">Body Mass Calculator</h1> </Col>
</Row>
<Row>
	<Col size="2"> <h2>Units</h2> </Col>
	<Col size="2"> <button on:click={toggle}> {impUnits ? 'Imperial' : 'Metric'} </button> </Col>
	<Col></Col>
</Row>
<Row>
	<Col size="2"> <h2>Values</h2> </Col>
	<Col size="2"> Height </Col>
	{#if impUnits}
	<Col size="2">	
	<input
		class="imp"
		value={hf}
		on:input={(e) => setFromHf(e.target.value)}
		type="number" />
	</Col>
	<Col size="1">	
		ft
	</Col>
	<Col size="2">	
		<input
		class="imp"
		value={hi}
		on:input={(e) => setFromHi(e.target.value)}
		type="number" />
	</Col>
	<Col size="1">	
	in
</Col>
{:else}
<Col size="2">	
		<input
		value={hm}
		on:input={(e) => setFromHm(e.target.value)}
		type="number" />
	</Col>
	<Col size="1">	
	cm
</Col>
{/if}

</Row>

<Row>
<Col size="2"> </Col>
<Col size="2"> Weight</Col>
<Col size="2"> 

{#if impUnits}
	<input value={w} on:input={(e) => setFromW(e.target.value)} type="number" />
{:else}
	<input
		value={wm}
		step="0.1"
		on:input={(e) => setFromWm(e.target.value)}
		type="number" />
	
{/if}
</Col>
<Col size="1">	
	{impUnits ? 'lbs' : 'kg'}
</Col>
</Row>
<Row>
<Col size="2"> </Col>
<Col size="2"> BMI</Col>
<Col size="2"> 
<input
	class:red={bmi > 26}
	class:blue={bmi < 21}
	step="0.1"
	value={bmi}
	on:input={(e) => setFromB(e.target.value)}
	type="number" />
</Col>
</Row>
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


<style>
	.blue{
		font-size:xx-large;
		color:blue;

	}
	.red{
		font-size: xx-large;
		color:crimson;
	}
</style>