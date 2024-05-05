<div class="background">
<div class="container">
    <h2 class = "forgot">Forgot Password</h2>
    <input bind:value={email} placeholder="Enter your email" type="email">
    <button class="btn btn-primary btn-block" on:click={fetchSecurityQuestion}>Submit</button>
    <button class="btn btn-error btn-block" on:click={goToSignIn}>Back to Sign in page</button>
    
<br><br><br><br>

    {#if showSecurityQuestion}
    <div>
        <p class="forgot">{securityQuestion}</p>
        <input bind:value={userAnswer} placeholder="Your answer" type="text">
        <input bind:value={newPassword} placeholder="New password" type="password">
        <input bind:value={confirmPassword} placeholder="Confirm new password" type="password">
        <button class="btn btn-success btn-block" on:click={verifyAnswerAndResetPassword}>Reset Password</button>
    </div>
    {/if}
</div>
</div>

<style>
    .background{
        background: linear-gradient(to bottom, #bdc3c7 0%, #2c3e50 100%);
        height: 820px;
        margin-top: -20px;
    }
    .forgot{
        color: #000;
        font-size: 30px;
        font-weight: bold;
    }
    .container {
        max-width: 400px;
        margin: 20px auto;
        padding: 20px;
        box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    }
    input, button {
        width: 100%;
        padding: 10px;
        margin-top: 10px;
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

    let email = "";
    let securityQuestion = "";
    let userAnswer = "";
    let newPassword = "";
    let confirmPassword = "";
    let showSecurityQuestion = false;

     // Validation function
     function validatePassword(password) {
        const hasLetter = /[a-zA-Z]/.test(password);
        const hasNumber = /\d/.test(password);
        const hasSpecial = /[^a-zA-Z\d]/.test(password);
        const isLongEnough = password.length >= 10;
        return hasLetter && hasNumber && hasSpecial && isLongEnough;
    }

    async function fetchSecurityQuestion() {
        const { data, error } = await supabase
            .from("users")
            .select("SecurityQuestion")
            .eq("email", email.toLowerCase())
            .single();

        if (error || !data) {
            alert("Email not found.");
            return;
        }

        securityQuestion = data.SecurityQuestion;
        showSecurityQuestion = true;
    }

    async function verifyAnswerAndResetPassword() {
        if (newPassword !== confirmPassword) {
            alert("Passwords do not match!");
            return;
        }

        if (!validatePassword(newPassword)) {
            alert("Password must be at least 10 characters long and include at least one letter, one number, and one special character.");
            return;
        }

        const { data, error } = await supabase
            .from("users")
            .select("SecurityAnswer")
            .eq("email", email.toLowerCase())
            .single();

        if (error || data.SecurityAnswer.toLowerCase() !== userAnswer.toLowerCase()) {
            alert("Incorrect security answer.");
            return;
        }

        // Update the password in the database
        const { error: updateError } = await supabase
            .from("users")
            .update({ password: newPassword }) 
            .eq("email", email.toLowerCase());

        if (updateError) {
            alert("Failed to update password.");
            return;
        }

        alert("Password updated successfully.");
        goto('/'); // Redirect to the login page
    }

    function goToSignIn() {
        goto ('/');
    }
</script>



