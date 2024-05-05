<div id="background">
<h1 class="header"><b>Are You sure you want to logout?</b></h1>
<div class="button-container">
    <button on:click={loggingOut} class="btn btn-success">Yes</button>
    <button on:click={loggedIn} class="btn btn-error">No</button>
</div>
<footer></footer>
</div>

<style>
#background{
    background: linear-gradient(to bottom, #bdc3c7 0%, #2c3e50 100%);
    min-height: 130vh; 
    display: flex;
    flex-direction: column;
    justify-content: center; 
    align-items: center; 
    margin-top: 0px; 
}
.header {
  font-size: 50px;
  text-align: center; 
  margin-top: -200px;
  margin-right: 20px;
  color: white;
}

.button-container {
  display: flex;
  justify-content: center; 
  width: 100%;
}

.btn {
  padding: 25px 25px;
  margin: 20px; 
  cursor: pointer; 
  font-size: 40px;
}



  </style>
  
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

    async function loggingOut() {

        // Clear local storage that stores session information
        localStorage.removeItem("token");
        localStorage.removeItem("email");

        // Redirect the user to the login page
        goto('/');
    }


    function loggedIn() {
        // Redirect the user to the dashboard
        goto('/dashboard');
    }


    </script>