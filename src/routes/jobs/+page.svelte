<select bind:value={item}>
    <option value="IT">IT</option>
    <option value="Accountant">Accountant</option>
    <option value="Customer Service">Customer Service</option>
    <option value="Waiter">Waiter</option>
</select>
<button on:click={showJob}>Submit</button>

<br><br>
<section style="position:relative; left:50%; transform:translateX(-50%)">

{#if IT}
<div class="card">
	<div class="card-body">
		<h2 class="card-header">IT Job - London</h2>
		<p class="text-content2">Are you looking for a job in this sector?</p>
		<div class="card-footer">
			<a href="https://uk.indeed.com/jobs?q=it&l=London%2C%20Greater%20London&from=searchOnDesktopSerp" target="_blank"><button class="btn-secondary btn">Learn More</button></a>
		</div>
	</div>
</div>
{/if}
<br>

{#if accountant}
<div class="card">
	<div class="card-body">
		<h2 class="card-header">Junior Accountant - Oxford</h2>
		<p class="text-content2">Are you looking for a job in this sector?</p>
		<div class="card-footer">
			<a href="https://uk.indeed.com/q-accounting-l-oxford-jobs.html" target="_blank"><button class="btn-secondary btn">Learn More</button></a>
		</div>
	</div>
</div>
{/if}
<br>

{#if customerService}
<div class="card">
	<div class="card-body">
		<h2 class="card-header">Customer Service - London</h2>
		<p class="text-content2">Are you looking for a job in this sector?</p>
		<div class="card-footer">
			<a href="https://uk.indeed.com/jobs?q=customer%20service&l=London%2C%20Greater%20London&from=searchOnHP" target="_blank"><button class="btn-secondary btn">Learn More</button></a>
		</div>
	</div>
</div>
{/if}
<br>

{#if waiter}
<div class="card">
	<div class="card-body">
		<h2 class="card-header">Waiter - London</h2>
		<p class="text-content2">Are you looking for a job in this sector?</p>
		<div class="card-footer">
			<a href="https://uk.indeed.com/jobs?q=waiter&l=London%2C%20Greater%20London&from=searchOnDesktopSerp" target="_blank"><button class="btn-secondary btn">Learn More</button></a>
		</div>
	</div>
</div>
{/if}
<br><br><br><br>
</section>
<BottomBar />
<script>
    import BottomBar from "$lib/BottomBar.svelte"
    import { onMount } from "svelte";
    import { goto } from "$app/navigation";
    import {createClient} from "@supabase/supabase-js";

    const supabase = createClient("https://tkkcqbnwdharnagmhten.supabase.co", "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InRra2NxYm53ZGhhcm5hZ21odGVuIiwicm9sZSI6ImFub24iLCJpYXQiOjE3MTEzOTk2MDMsImV4cCI6MjAyNjk3NTYwM30.VyF3VVorlbPus_GBz8YXK4c-xQ3jInSyxiNSTndDTpg")
  
    onMount(()=>
    {
       autoLogin();

    }
    );

    async function autoLogin()
    {
        if(localStorage.getItem("token")
        && localStorage.getItem("email"))
        {
            let checkDetails = await supabase
            .from("users")
            .select()
            .match({email:localStorage.getItem("email"), 
            sessionToken: localStorage.getItem("token")})

            console.log(checkDetails.data)

            if(checkDetails.data.length > 0)
            {
               
                let getFirstName = await supabase
                .from("users")
                .select()
                .match({email: localStorage.getItem("email")})

                fname = getFirstName.data[0].FirstName;

            } else
            {
                goto("/")
            }
        } else
        {
            
            goto("/")
        }
    }



    let item = "";
    let accountant = true;
    let IT = true;
    let customerService = true;
    let waiter = true;
   
    function showJob()
    {
        if(item.toLowerCase() == "accountant")
        {
            IT = false;
            accountant = true;
            customerService = false;
            waiter = false;
        }

        if(item == "IT")
        {
            accountant = false;
            IT = true;
            customerService = false;
            waiter = false;
        }

        if(item == "Customer Service")
        {
            accountant = false;
            IT = false;
            customerService = true;
            waiter = false;
        }

        if(item == "Waitor")
        {
            accountant = false;
            IT = false;
            customerService = false;
            waiter = true;
        }
    }

</script>