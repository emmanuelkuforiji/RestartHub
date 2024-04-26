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
            <div><br>
                <label>Email address</label>
                <input bind:value={emailVar} placeholder="example@hotmail.com" type="email" class="input max-w-full">
            </div>
            <div><br>
                <label>Password</label>
                <input id="passbox" bind:value={passwordVar} placeholder="Type here" type="password" class="input max-w-full">
            </div>
            <div class="forgot-password">
                <a on:click={goToForgotPassword} class="sign-up-link">Forgot your password?</a>
            </div>
            <div class="form-action">
                <button on:click={login} class="max-w-full">Sign in</button>
            </div>
            <div class="sign-up">
                <a on:click={goToRegister} class="sign-up-link">Don't have an account yet? Sign up.</a>
            </div>
        </div>
    </div>
</div>

<style>

#background {
    position: absolute;
    width: 100%;
    height: 1000px;
    background: linear-gradient(to bottom, #bdc3c7 0%, #2c3e50 100%);

}

.container {
    position: relative;
    top: 100px;
    color: white;
    width: 450px;
    max-width: 89%; 
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

.input {
    position:relative;
    width: 100%;
    margin-left: 7px;
}

.forgot-password, .sign-up {
    text-align: center;
    margin-top: 10px;
}

.form-action button {
    width: 96%;
    margin-left: 7px;
    background-color: black; 
    color: white;
    padding: 10px;
    border: none;
    cursor: pointer;
}

.form-action button:hover {
    background-color: #5ab300; 
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
    cursor: pointer;
}

</style>

<script>
    // Import necessary functions and modules
    import { createClient } from "@supabase/supabase-js"; // Imports Supabase client for database interaction
    import { goto } from "$app/navigation"; // Imports goto for navigation within Svelte app
    import { onMount } from "svelte"; // Imports onMount to run code when the component mounts
  
    // Initialize the Supabase client
    const supabase = createClient(
        "https://tkkcqbnwdharnagmhten.supabase.co", 
        "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InRra2NxYm53ZGhhcm5hZ21odGVuIiwicm9sZSI6ImFub24iLCJpYXQiOjE3MTEzOTk2MDMsImV4cCI6MjAyNjk3NTYwM30.VyF3VVorlbPus_GBz8YXK4c-xQ3jInSyxiNSTndDTpg"
    );

    // Variable to track if login has failed
    let isLoginFailed = false;

    // Variables for form inputs
    let emailVar = "";
    let passwordVar = "";

    // onMount lifecycle hook to check session on component mount
    onMount(() => {
       autoLogin(); // Attempt to auto-login the user
        const textinput = document.getElementById("passbox");

        textinput.addEventListener("keydown", (event)=> {
            if(event.key == "Enter")
            {
                login();
            }
        });
    });

    // Auto-login function
    async function autoLogin() {
        // Check if token and email are stored in localStorage
        if(localStorage.getItem("token") && localStorage.getItem("email")) {
            // Attempt to fetch user details from Supabase to validate session
            let checkDetails = await supabase
                .from("users")
                .select()
                .match({email: localStorage.getItem("email"), sessionToken: localStorage.getItem("token")});

            // If user details are found, navigate to the dashboard
            if(checkDetails.data.length > 0) {
                goto("/dashboard");
            }
        }
    }

    // Function to handle user login
    async function login() {
    try {
        let getUsers = await supabase
            .from("users")
            .select()
            .match({email: emailVar.toLowerCase(), password: passwordVar}); // Consider hashing

        if(getUsers.data.length > 0) {
            let randomChar = ["DBHS", "EW-D", "Q@&D", "#.EQ"];
            let token = Math.floor(Math.random() * 900000000) + 100000000 + randomChar[Math.floor(Math.random() * 4)];

            let setToken = await supabase
                .from("users")
                .update({sessionToken: token})
                .match({email: emailVar.toLowerCase()});

            localStorage.setItem("token", token);
            localStorage.setItem("email", emailVar.toLowerCase());
            
            goto("/dashboard");
        } else {
            isLoginFailed = true;
        }
    } catch (error) {
        console.error("Login error:", error);
        isLoginFailed = true;
    }
}

    // Function to navigate to the registration page
    function goToRegister() {
        goto('/register'); 
    }

    // Function to navigate to forgot password page
    function goToForgotPassword() {
        goto('/forgot_password')
    }

</script>
