<select bind:value={item}>
    <option value="IT">IT</option>
    <option value="Accountant">Accountant</option>
</select>
<button on:click={showJob}>Submit</button>

<br><br>
<section style="position:relative; left:50%; transform:translateX(-50%)">

{#if IT}
<div class="card">
	<div class="card-body">
		<h2 class="card-header">IT Job - London</h2>
		<p class="text-content2">Are you looking to increase your productivity at work?</p>
		<div class="card-footer">
			<button class="btn-secondary btn">Learn More</button>
		</div>
	</div>
</div>
{/if}
<br>

{#if accountant}
<div class="card">
	<div class="card-body">
		<h2 class="card-header">Junior Accountant - Oxford</h2>
		<p class="text-content2">Are you looking to increase your productivity at work?</p>
		<div class="card-footer">
			<a href="https://uk.indeed.com/q-accounting-l-oxford-jobs.html" target="_blank"><button class="btn-secondary btn">Learn More</button></a>
		</div>
	</div>
</div>
{/if}

</section>

<script>
    let item = "";
    let accountant = true;
    let IT = true;
   
    function showJob()
    {
        if(item.toLowerCase() == "accountant")
        {
            IT = false;
            accountant = true;
        }

        if(item == "IT")
        {
            accountant = false;
            IT = true;
        }
    }

</script>