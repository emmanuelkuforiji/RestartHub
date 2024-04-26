<div id="background">
    <div class="container">
        <div class="header">
            <h1>Sign Up</h1>
            <p>Create your account</p>
        </div>

        <div class="form-group">
            <div class="form-field">
                <label>Email address</label>
                <input bind:value={emailVar} placeholder="example@hotmail.com" type="email" class="input max-w-full">
            </div>
            <div class="form-field">
                <label>Username</label>
                <input bind:value={usernameVar} placeholder="Username" type="text" class="input max-w-full">
            </div>
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
            <div class="form-field">
                <label>Security Question</label>
                <select bind:value={selectedSecurityQuestion} class="input max-w-full">
                    <option value="" disabled selected>{selectedSecurityQuestion ? 'Pick a question' : ''}</option>
                    {#each securityQuestions as question}
                        <option value={question}>{question}</option>
                    {/each}
                </select>
            </div>
            <div class="form-field">
                <label>Security Answer</label>
                <input bind:value={securityAnswerVar} placeholder="Security Answer" type="text" class="input max-w-full">
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
    height: 1000px;
    background: linear-gradient(to bottom, #bdc3c7 0%, #2c3e50 100%);
}

.container {
    position: relative;
    top: 30px;
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
    background-color: green; 
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
    let usernameVar = "";
    let firstNameVar = "";
    let surnameVar = "";
    let passwordVar = "";
    let confirmPasswordVar = "";
    let securityAnswerVar = "";

    // Array of predefined security questions
    const securityQuestions = [
        'What is your oldest siblings middle name?', 
        'What was the first concert you attended?', 
        'What was the make and model of your first car?', 
        'In what city or town did your parents meet?', 
        'What was the name of your first childhood friend?', 
        'What is your grandmothers maiden name?',
        'What was the name of your favourite school teacher?',
        'What was your childhood best friends nickname?'  
    ];

    // Reactive variable to hold the selected security question
    let selectedSecurityQuestion = "";

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
        if (!emailVar || !firstNameVar || !surnameVar || !passwordVar || !confirmPasswordVar || !selectedSecurityQuestion || !securityAnswerVar) {
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

        // Check if the username is already taken
        const { data: userData, error: userError } = await supabase
            .from("users")
            .select("username")
            .eq("username", usernameVar)
            .single();

        if (userData) {
            alert("The username is already taken. Please choose another one.");
            return;
        }

        // Check Security Question has been selected
        if (!selectedSecurityQuestion) {
        alert("Please select a security question.");
        return;
        }

        // Convert to lowercase
        emailVar = emailVar.toLocaleLowerCase();
        usernameVar = usernameVar.toLocaleLowerCase();
        securityAnswerVar = securityAnswerVar.toLowerCase();

        // Insert new user data into the database
        const { data, error } = await supabase
            .from("users")
            .insert([
                { email: emailVar, username: usernameVar, FirstName: firstNameVar, LastName: surnameVar, password: passwordVar, SecurityQuestion: selectedSecurityQuestion, SecurityAnswer: securityAnswerVar },
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
