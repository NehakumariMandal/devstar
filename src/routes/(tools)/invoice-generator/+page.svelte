<script lang="ts">
	import { Label, Input, Range } from 'flowbite-svelte';
	import Intro from '$lib/Intro.svelte';

	export let data;

	let background = '#e0e0e0';
	let size = 250;
	let radius = 50;
	let distance = 20;
	let intensity = 0.15;
	let blur = 60;

	let inputSets: any[] = [{ item: '', rate: '', quantity: 1, additionalInfo: '', taxRate: 0 }];
	let index = -1;
	let showFirstPair = true;
	let moveDescription = false;
	let showTaxInfo = false;

	let subtotal = 0;
	let tax = 0;
	let total = 0;
	let balanceDue = 0;

	function updateTotals() {
		subtotal = inputSets.reduce((acc, inputSet) => {
			const itemAmount = inputSet.quantity * inputSet.rate || 0;
			const itemTax = itemAmount * (inputSet.taxRate / 100);
			inputSet.tax = itemTax; // Store tax for each item
			return acc + itemAmount;
		}, 0);

		tax = inputSets.reduce((acc, inputSet) => {
			return acc + inputSet.tax || 0;
		}, 0);

		total = subtotal + tax;
		balanceDue = total;
	}

	function addInputSet() {
		index += 1;
		inputSets = [
			...inputSets,
			{ id: index, item: '', rate: '', quantity: 1, additionalInfo: '', taxRate: 0 }
		];
		showFirstPair = true;
		moveDescription = true;
		updateTotals();
	}

	function deleteInputSet(idToDelete) {
		const indexToDelete = inputSets.findIndex((inputSet) => inputSet.id === idToDelete);
		if (indexToDelete !== -1) {
			index -= 1;
			inputSets.splice(indexToDelete, 1);
			updateTotals();
		}
	}
</script>

<Intro heading={data.meta.title} description={data.meta.description} />

<section class="bg-white dark:bg-gray-900">
	<div class="mx-auto max-w-screen-xl lg:px-12">
		<div class="card">
			<hr class="border-t border-gray-400" />
			<div id="invoice">
				<table class="w-full">
					<thead class="mb-7">
						<tr class="w-full h-10">
							<th class="w-10" />
							<th class="text-start w-2/5">Description</th>
							<th class="text-end w-1/10 px-3">Rate</th>
							<th class="text-end w-1/10 px-3">Quantity</th>
							<th class="text-center w-fit">Amount</th>
							<th class="text-center w-1/6">Tax(%)</th>
						</tr>
						<tr>
							<td class="border-t border-black" colspan="6">
								<hr />
							</td>
						</tr>
					</thead>
					<tbody>
						{#each inputSets as inputSet}
							{#if showFirstPair || index > 0}
								<tr>
									<td>
										<button
											class="w-10 h-10 font-bold border text-xl bg-gray-400 rounded-lg m-3"
											on:click={() => deleteInputSet(inputSet.id)}>X</button
										>
									</td>
									<td>
										<input
											class="w-full rounded"
											type="text"
											min="0"
											placeholder="Item Description"
											bind:value={inputSet.item}
										/>
									</td>
									<td class="px-2">
										<input
											class="w-full text-end rounded"
											type="number"
											min="0"
											placeholder="0.00"
											bind:value={inputSet.rate}
											on:change={updateTotals}
										/>
									</td>
									<td class="px-1">
										<input
											class="w-full rounded text-end"
											type="number"
											min="0"
											bind:value={inputSet.quantity}
											on:change={updateTotals}
										/>
									</td>
									<td>
										<p class="flex justify-center text-2xl">₹{inputSet.quantity * inputSet.rate}</p>
									</td>
									<td class="text-center">
										<input
											class="w-16 text-end rounded"
											type="number"
											min="0"
											bind:value={inputSet.taxRate}
											on:change={updateTotals}
										/>
									</td>
								</tr>
								<tr>
									<td colspan="6">
										<textarea
											class="w-2/5 ml-16 rounded h-36 text-start pt-2 resize-none"
											placeholder="Additional Details"
											bind:value={inputSet.additionalInfo}
										/>
									</td>
								</tr>
								{#if index !== inputSets.length - 1}
									<tr>
										<td class="py-3" colspan="6">
											<hr />
										</td>
									</tr>
								{/if}
							{/if}
						{/each}
					</tbody>
				</table>
			</div>
			<button
				class="w-10 h-10 border font-bold text-3xl text-white bg-gray-600 rounded-lg shadow-lg m-3"
				on:click={addInputSet}>+</button
			>
			<hr class="py-5" />
			<div
				class="card gap-12 items-center mx-auto max-w-screen-xl overflow-hidden rounded-lg lg:grid lg:grid-cols-2"
			>
				<div class="p-8 h-full" />
				<div class="p-8 h-full">
					<div class="mb-4 text-gray-700">
						<div class="invoice-summary-label">
							Subtotal: <span class="price">{subtotal.toFixed(2)}</span>
						</div>
						<div class="invoice-summary-label">
							Tax: <span class="price">{tax.toFixed(2)}</span>
						</div>
						<div class="invoice-summary-label">
							Total: <span class="price">{total.toFixed(2)}</span>
						</div>
						<div class="invoice-summary-label">
							Balance due: <span class="price">{balanceDue.toFixed(2)}</span>
						</div>
					</div>
				</div>
			</div>
		</div>
	</div>
</section>

<style>
	:is(.dark .card) {
		box-shadow: rgba(255, 255, 255, 0.5) 0 0 0 2px;
		background-color: white;
	}
	.card {
		box-shadow: rgba(0, 0, 0, 0.1) 0 0 0 2px;
	}
</style>
