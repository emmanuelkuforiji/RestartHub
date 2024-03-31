<div id="background">
    <div class="container">
        <div class="header">
            <h1>Sign In</h1>
            <p>Sign in to access your account</p>
        </div>

        {#if isLoginFailed == true}
        <div class="alert alert-error">
            <svg width="48" height="48" viewBox="0 0 48 48" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path fill-rule="evenodd" clip-rule="evenodd" d="M24 4C12.96 4 4 12.96 4 24C4 35.04 12.96 44 24 44C35.04 44 44 35.04 44 24C44 12.96 35.04 4 24 4ZM24 26C22.9 26 22 25.1 22 24V16C22 14.9 22.9 14 24 14C25.1 14 26 14.9 26 16V24C26 25.1 25.1 26 24 26ZM26 34H22V30H26V34Z" fill="#E92C2C" />
            </svg>
            <div>
                <span>Error</span>
                <span>Your username or password was not recognised.</span>
            </div>
        </div>
        {/if}

        <div class="form-group">
            <div class="form-field">
                <label>Email address</label>
                <input bind:value={emailVar} placeholder="example@hotmail.com" type="email" class="input">
            </div>
            <div class="form-field">
                <label>Password</label>
                <input bind:value={passwordVar} placeholder="Type here" type="password" class="input">
            </div>
            <div class="forgot-password">
                <a>Forgot your password?</a>
            </div>
            <div class="form-action">
                <button on:click={login}>Sign in</button>
            </div>
            <div class="sign-up">
                <a on:click={goToRegister} href="javascript:void(0);" class="sign-up-link">Don't have an account yet? Sign up.</a>
            </div>
        </div>
    </div>
</div>

<style>

#background {
    position: absolute;
    width: 100%;
    height: 100%;
    background: rgb(63,94,251);
    background: linear-gradient(167deg, rgba(63,94,251,1) 0%, rgba(252,70,107,1) 100%);
}

.container {
    position: relative;
    top: 100px;
    color: white;
    width: 100%;
    max-width: 450px; 
    margin: auto;
    display: flex;
    flex-direction: column;
    gap: 6px;
}

.header h1 {
    text-align: center;
    font-size: 3rem;
    font-weight: bold;
}

.header p {
    text-align: center;
    font-size: 1rem;
}

.alert {
    width: 98%;
    margin-left: 3px;
}

.form-field label {
    color: white;
    margin-left: 4px;
}

.input {
    width: 96%;
    margin-left: 7px;
}

.forgot-password, .sign-up {
    text-align: center;
    margin-top: 10px;
}

.form-action button {
    width: 96%;
    margin-left: 7px;
    background-color: #007bff; 
    color: white;
    padding: 10px;
    border: none;
    cursor: pointer;
}

.form-action button:hover {
    background-color: #0056b3; 
}

.sign-up-link {
    color: white;
    text-decoration: none;
    padding: 5px;
    border-radius: 5px; 
    transition: background-color 0.3s, color 0.3s; 
    background-color: transparent; 
}

.sign-up-link:hover {
    background-color: #0056b3; 
    text-decoration: underline; 
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

    }

    function goToRegister() {
        goto('/register'); // Navigate to the register page
    }

    let emailVar = "";
    let passwordVar = "";
</script>