<div id="background">
    <div class="container">
        <div class="header">
            <h1>Sign Up</h1>
            <p>Create your account</p>
        </div>

        <div class="form-group">
            <div class="form-field">
                <label>Email address</label>
                <input bind:value={emailVar} placeholder="example@hotmail.com" type="email" class="input">
            </div>
            <div class="form-field">
                <label>First Name</label>
                <input bind:value={firstNameVar} placeholder="First Name" type="text" class="input">
            </div>
            <div class="form-field">
                <label>Surname</label>
                <input bind:value={surnameVar} placeholder="Surname" type="text" class="input">
            </div>
            <div class="form-field">
                <label>Password</label>
                <input bind:value={passwordVar} placeholder="Type here" type="password" class="input">
            </div>
            <div class="form-field">
                <label>Confirm Password</label>
                <input bind:value={confirmPasswordVar} placeholder="Type here" type="password" class="input">
            </div>
            <div class="form-action">
                <button on:click={register}>Sign Up</button>
            </div>
            <div class="sign-in">
                <a on:click={goToSignIn} class="sign-in-link">Already have an account? Sign In.</a>
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
    background-color: #007bff;
    color: white;
    padding: 10px;
    border: none;
    cursor: pointer;
}

.form-action button:hover {
    background-color: #0056b3; 
}

</style>

<script>
    // Import functions from external libraries
    import { createClient } from "@supabase/supabase-js"; // Imports createClient from supabase to interact with Supabase database
    import { goto } from "$app/navigation"; // Imports goto from svelte navigation for programmatic navigation

    // Initialise and setup Supabase client
    const supabase = createClient("https://tkkcqbnwdharnagmhten.supabase.co", "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InRra2NxYm53ZGhhcm5hZ21odGVuIiwicm9sZSI6ImFub24iLCJpYXQiOjE3MTEzOTk2MDMsImV4cCI6MjAyNjk3NTYwM30.VyF3VVorlbPus_GBz8YXK4c-xQ3jInSyxiNSTndDTpg");

    // Declare reactive variables for form inputs
    let emailVar = "";
    let firstNameVar = "";
    let surnameVar = "";
    let passwordVar = "";
    let confirmPasswordVar = "";

    // Utility functions for input validation
    
    // Validates email format
    function validateEmail(email) {
        return email.match(
            /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
        );
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
    async function register() {
        // Check if all fields are filled
        if (!emailVar || !firstNameVar || !surnameVar || !passwordVar || !confirmPasswordVar) {
            alert("All fields must be filled out.");
            return;
        }

        // Perform input validations
        if (!validateEmail(emailVar)) {
            alert("Please enter a valid email address.");
            return;
        }

        if (!validateName(firstNameVar) || !validateName(surnameVar)) {
            alert("First name and surname must contain letters only.");
            return;
        }

        if (passwordVar !== confirmPasswordVar) {
            alert("Passwords do not match!");
            return;
        }

        if (!validatePassword(passwordVar)) {
            alert("Password must be at least 10 characters long and contain at least one letter, one number, and one special character.");
            return;
        }

        // Insert new user data into the database
        const { data, error } = await supabase
            .from("users")
            .insert([
                { email: emailVar, FirstName: firstNameVar, LastName: surnameVar, password: passwordVar },
            ]);

        // Handle response
        if (error) {
            alert("Registration error: " + error.message);
        } else {
            alert("You have successfully registered");
            goToSignIn(); // Redirect to the sign-in page upon successful registration
        }
    }

    // Navigation function to redirect user to the sign-in page
    function goToSignIn() {
        goto("/"); 
    }
</script>
