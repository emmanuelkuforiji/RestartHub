<div style="position:absolute; width:100%; height:100%; 
background: rgb(63,94,251);
background: linear-gradient(167deg, rgba(63,94,251,1) 0%, rgba(252,70,107,1) 100%);">


<div class="mx-auto flex w-full max-w-sm flex-col gap-6"
style="position:relative; top:100px; color:white;">
	<div class="flex flex-col items-center">
		<h1 class="text-3xl font-semibold">Sign In</h1>
		<p class="text-sm">Sign in to access your account</p>
	</div>

    <!--ALERT-->
    {#if isLoginFailed == true}
    <div class="alert alert-error" style="width:98%; margin-left:3px">
        <svg width="48" height="48" viewBox="0 0 48 48" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path fill-rule="evenodd" clip-rule="evenodd" d="M24 4C12.96 4 4 12.96 4 24C4 35.04 12.96 44 24 44C35.04 44 44 35.04 44 24C44 12.96 35.04 4 24 4ZM24 26C22.9 26 22 25.1 22 24V16C22 14.9 22.9 14 24 14C25.1 14 26 14.9 26 16V24C26 25.1 25.1 26 24 26ZM26 34H22V30H26V34Z" fill="#E92C2C" />
        </svg>
        <div class="flex flex-col">
            <span style="color:black;">Error</span>
            <span class="text-content2">Your username or password was not recognised.</span>
        </div>
    </div>
    {/if}

	<div class="form-group">
		<div class="form-field">
			<label class="form-label" style="color:white;">Email address</label>

			<input bind:value={emailVar} 
            placeholder="example@hotmail.com" 
            type="email" class="input max-w-full" 
            style="max-width:96%; margin-left:7px;"/>
			
		</div>
		<div class="form-field">
			<label class="form-label" style="color:white;">Password</label>
			<div class="form-control">
				<input bind:value={passwordVar} 
                placeholder="Type here" type="password" 
                class="input max-w-full" 
                style="max-width:96%; margin-left:7px;"/>
			</div>
		</div>
		<div class="form-field">
			<div class="form-control justify-between">
				<div class="flex gap-2" style="margin-left: 7px;">
					<input type="checkbox" class="checkbox" />
					<a href="#">Remember me</a>
				</div>
				<label class="form-label">
					<a class="link link-underline-hover link-success text-sm">Forgot your password?</a>
				</label>
			</div>
		</div>
		<div class="form-field pt-5">
			<div class="form-control justify-between">
				<button 
                on:click={login} 
                class="btn btn-primary w-full"
                style="max-width:96%; margin-left:7px;">Sign in</button>
			</div>
		</div>

		<div class="form-field">
			<div class="form-control justify-center">
				<a class="link link-underline-hover link-success text-sm">Don't have an account yet? Sign up.</a>
			</div>
		</div>
	</div>
</div>


</div>

<style>
 .outfitfont{
  font-family: "Outfit", sans-serif;
  font-optical-sizing: auto;
  font-weight: 300;
  font-style: normal;
}

.mainH
{
    position:relative;
    font-size: 38px;
    color: white;
    left:50%;
    transform: translateX(-50%);
    top:80px;
    width:100px;
}
</style>

<script>
    import {createClient} from "@supabase/supabase-js";
    import { goto } from "$app/navigation";
    import { onMount } from "svelte";
  

    const supabase = createClient("https://tkkcqbnwdharnagmhten.supabase.co", "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InRra2NxYm53ZGhhcm5hZ21odGVuIiwicm9sZSI6ImFub24iLCJpYXQiOjE3MTEzOTk2MDMsImV4cCI6MjAyNjk3NTYwM30.VyF3VVorlbPus_GBz8YXK4c-xQ3jInSyxiNSTndDTpg")
    let isLoginFailed = false;


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

            if(checkDetails.data.length > 0)
            {
                goto("/dashboard");
            }
        }
    }

    async function login()
    {
        //async means asynchrous, it just means it will run
        //in a separate memory threat rather than blocking
        //the memory/current threat
        let getUsers = await supabase
        .from("users")
        .select()
        .match({email: emailVar, password: passwordVar})

        if(getUsers.data.length > 0)
        {
            let randomChar = ["DBHS", "EW-D", "Q@&D", "#.EQ"];
            //generate random token
            let token = Math.floor(Math.random() * 900000000) + 100000000
            console.log();

            let setToken = await supabase
            .from("users")
            .update({sessionToken: token.toString() + randomChar[Math.floor(Math.random() * 4)]})
            .match({email: emailVar})

            localStorage.setItem("token", token.toString() + randomChar[Math.floor(Math.random() * 4)])
            localStorage.setItem("email", emailVar);
            
            goto("/dashboard");

        } else
        {
            isLoginFailed = true;
        }

        /*
        for(let i =0; i < getUsers.data.length; i++)
        {
            if(getUsers.data[i].email == email
            && getUsers.data[i].password == password)
            {
                alert("login successful!");
            } else
            {
                alert("login failed!");
            }
        }
        */
    }

    let emailVar = "";
    let passwordVar = "";
</script>