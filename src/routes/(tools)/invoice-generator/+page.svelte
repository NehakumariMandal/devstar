<script lang="ts">
	
	import Intro from '$lib/Intro.svelte';
	
	export let data;
	
	let value;
	
	let background = '#e0e0e0';
	let size = 250;
	let radius = 50;
	let distance = 20;
	let intensity = 0.15;
	let blur = 60;

	let inputSets: any[] = [];
	let index = -1;
	let showFirstPair = false;
	let moveDescription = false;

	let subtotal = 0;
	let tax = 0;
	let total = 0;
	let balanceDue = 0;

	let  logo_image,
	let  fileinput;
	
	const onFileSelected =(e)=>{
  	let image = e.target.files[0];
            let reader = new FileReader();
            reader.readAsDataURL(image);
            reader.onload = e => {
                 logo_image = e.target.result
            };
}
  
</script>

<Intro heading={data.meta.title} description={data.meta.description} />

<section class="bg-white dark:bg-gray-900">
	
		<div class="invoice-detail-title content-block mx-auto max-w-screen-xl lg:px-12">
			<div class="invoice-title ">
			<h2>
			  <input type="text" id="invoice-title" name="invoice-title" placeholder="Invoice"  value="">		
			</h2>
			</div>
			<div class="logo">
					<div class="chan" on:click={()=>{fileinput.click();}}></div>
					<input style="display:none" type="file" accept=".jpg, .jpeg, .png" on:change={(e)=>onFileSelected(e)} bind:this={fileinput} >
					{#if logo_image}
					<img class="logo_image" src="{logo_image}" alt="" on:click={()=>{fileinput.click();}} />
					{:else}
					<img class="logo_image" src="https://static.thenounproject.com/png/625182-200.png" alt="" on:click={()=>{fileinput.click();}} />
					<span>Add Logo</span>
					{/if}
					
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
.invoice-detail-title
{
	box-shadow: rgba(255, 255, 255, 0.5) 0 0 0 2px;
	background-color: whitesmoke;
}

	.logo{
		display:flex;
		align-items:center;
		justify-content:center;
		flex-flow:column;
		float: right;
		margin-right: 10px;
		margin-bottom: 10px;
		margin-top:1em;
		background-color: white;
		}
	.invoice-title
	{	display:inline-block;
		align-items:center;
		justify-content:center;
		flex-flow:column;
		float: left ;
		margin-top:3em;
		margin-left: 10px;
	}
	.logo_image{
		display:flex;
		height:125px;
		width:200px;
	}
	

</style>
