<div class="background">
<p class="uName">Edit Profile</p>
<br>
<div class="form-group">
    <div class="form-field">
        <label>First Name</label>
        <input bind:value={firstNameVar} placeholder="First Name" type="text" class="input max-w-full">
    </div>
    <div class="form-field">
        <label>Surname</label>
        <input bind:value={surnameVar} placeholder="Surname" type="text" class="input max-w-full">
    </div>
    <div class="form-field">
        <label>Password</label>
        <input bind:value={passwordVar} placeholder="Type here" type="password" class="input max-w-full">
    </div>
    <div class="form-field">
        <label>Confirm Password</label>
        <input bind:value={confirmPasswordVar} placeholder="Type here" type="password" class="input max-w-full">
    </div>
    <div class="form-action">
        <button on:click={saveProfile}>Save</button>
    </div>
    <div class="btn btn-error">
        <button on:click={delAcc}>Delete Account</button>
    </div>
</div>
</div>

<style>

    .background {
        background: linear-gradient(to bottom, #bdc3c7 0%, #2c3e50 100%);
        height: 750px;
        margin-top: -10px;
    }

    .uName {
        position:relative;
        margin-top:10px;
        margin-left:10px;
        width: 90%;
        font-size: 28px;
        font-weight: bold;
        text-align: center;
        color: white;
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
    
    .form-field label {
        color: white;
        margin-left: 4px;
    }
    
    .input {
        width: 96%;
        margin-left: 7px;
    }
    
    .sign-in {
        text-align: center;
        margin-top: 10px;
    }
    
    .sign-in-link {
        color: white;
        text-decoration: none;
        padding: 5px;
        border-radius: 5px; 
        transition: background-color 0.3s, color 0.3s; 
        background-color: transparent; 
    }
    
    .sign-in-link:hover {
        background-color: #0056b3; 
        text-decoration: underline; 
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
        background-color: #0056b3; 
    }
    
    </style>

<BottomBar />

<script>
    import BottomBar from "$lib/BottomBar.svelte"
    import { onMount } from "svelte";
    import { goto } from "$app/navigation";
    import {createClient} from "@supabase/supabase-js";

    const supabase = createClient("https://tkkcqbnwdharnagmhten.supabase.co", "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InRra2NxYm53ZGhhcm5hZ21odGVuIiwicm9sZSI6ImFub24iLCJpYXQiOjE3MTEzOTk2MDMsImV4cCI6MjAyNjk3NTYwM30.VyF3VVorlbPus_GBz8YXK4c-xQ3jInSyxiNSTndDTpg")
    
    // Declare reactive variables for form inputs
    let firstNameVar = "";
    let surnameVar = "";
    let passwordVar = "";
    let confirmPasswordVar = "";
    let UID;


    let fname;

    onMount(()=>
    {
       autoLogin();
       populatefields();
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
                UID = getFirstName.data[0].username;

            } else
            {
                goto("/")
            }
        } else
        {
            
            goto("/")
        }
    }

    async function populatefields()
    {
        let checkDetails = await supabase
                .from("users")
                .select()
                .match({email: localStorage.getItem("email"), sessionToken: localStorage.getItem("token")});
        
        firstNameVar = checkDetails.data[0].FirstName;
        surnameVar = checkDetails.data[0].LastName;
        passwordVar = checkDetails.data[0].password;
        confirmPasswordVar = checkDetails.data[0].password;
    }


    // Validates name to ensure it contains letters only
    function validateName(name) {
        return /^[A-Za-z]+$/.test(name);
    }
    

    // Validates password for minimum length, inclusion of letters, numbers, and special characters
    function validatePassword(password) {
        const hasLetter = /[a-zA-Z]/.test(password);
        const hasNumber = /\d/.test(password);
        const hasSpecial = /[^a-zA-Z\d]/.test(password);
        const isLongEnough = password.length >= 10;
        return hasLetter && hasNumber && hasSpecial && isLongEnough;
    }

    // The main function to handle user registration
    async function saveProfile() 
    {

        if (!validateName(firstNameVar) || !validateName(surnameVar)) {
            alert("First name and surname must contain letters only.");
            return;
        }

        else if (passwordVar !== confirmPasswordVar) {
            alert("Passwords do not match!");
            return;
        }

        else if (!validatePassword(passwordVar)) {
            alert("Password must be at least 10 characters long and contain at least one letter, one number, and one special character.");
            return;
        } else
        {
            let {checkDetails, error} = await supabase
                .from("users")
                .update({FirstName: firstNameVar, 
                LastName: surnameVar, password: passwordVar})
                .match({email: localStorage.getItem("email"), sessionToken: localStorage.getItem("token")});

                 // Handle response
                if (error) {
                    alert("No connection detected");
                } else {
                    alert("You have successfully updated your profile");
                }
        }
    }

    async function delAcc() {
        goto('/delete');
    }

    </script>