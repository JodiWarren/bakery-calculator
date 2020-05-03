
<style>
	.line {
		display: flex;
		width: 100%;
		align-items: baseline;
	}
	.line > * {
		margin-right: 10px;
	}
	
	.line__name {
		width: 50px;
	}
</style>

<script>
const getRandomId = () => Math.floor((Math.random() * Date.now())/1e5).toString();
		
let targetAmount = 520;
let unit = "g";
	
let items = [
	{
		name: "Flour",
		amount: 100,
		id: "123",
	},
	{
		name: "Water",
		amount: 75,
		id: "4625",
	},
	{
		name: "Starter",
		amount: 20,
		id: "1234",
	},
	{
		name: "Salt",
		amount: 2.75,
		id: "1673765",
	},
	{
		name: "Oil",
		amount: 2,
		id: "4132",
	},
]

function addItem(e) {
	const newName = e.target.elements[0].value;
	const newValue = {
		name: newName,
		amount: 0,
		id: getRandomId()
	};
	items = [...items, newValue];
}

function removeItem(e) {
	const targetIndex = items.findIndex(item => item.id === e.target.dataset.item);
	items = [...items.slice(0, targetIndex), ...items.slice(targetIndex + 1)]
}
	
const toTwoPoints = (x) => Number.parseFloat(x).toFixed(2);

function calcAmount(thisAmount, targetAmount){
	const listTotal = items.reduce((acc, curVal) => acc + Number(curVal.amount), 0);
	const percent = thisAmount/listTotal;
	const finalAmount =  percent * targetAmount;
	return `${toTwoPoints(finalAmount)}${unit}`;
}
</script>

<h1>
	Bakery Ratio Calculator
</h1>
<form>
	<div class="target-container">
		<span class="line__name">Target amount:</span> 
		<input class="target" type="number" bind:value={targetAmount}>
	</div>
	<div class="line">
		<span class="line__name">Unit:</span>
		<input class="line__amount" bind:value={unit}>
	</div>
	{#each items as {name, amount, id} (id)}
		<div class="line">
			<span class="line__name">{name}:</span>
			<input class="line__amount" bind:value={amount}>
			<button class="remove-item" data-item={id} on:click={removeItem}>
				N
			</button>
		</div>
	{/each}
</form>

<hr>

<form on:submit|preventDefault={addItem}>
	<input name="name">
	<button type="submit">
		Add Line
	</button>
</form>

<hr>

<pre>
	{#each items as {name, amount, id} (id)}
		{name}: {calcAmount(amount, targetAmount, unit)}<br>
	{/each}
</pre>