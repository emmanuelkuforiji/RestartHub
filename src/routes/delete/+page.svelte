<div id="background">
    <h1 class="header"><b>Are you sure you want to DELETE your account?</b></h1>
    <div class="button-container">
        <button on:click={delAccount} class="btn btn-success">Yes</button>
        <button on:click={keepAccount} class="btn btn-error">No</button>
    </div>
    <footer></footer>
    </div>
    
    <style>
    #background{
        background: rgb(63,94,251);
        background: linear-gradient(167deg, rgba(63,94,251,1) 0%, rgba(252,70,107,1) 100%);
        min-height: 100vh; 
        display: flex;
        flex-direction: column;
        justify-content: center; 
        align-items: center; 
        margin: 0; 
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

        
    
    function keepAccount() {
        // Redirect the user to the profile
        goto('/profile');
    }

    async function delAccount() {
        let delAccount = await supabase
        .from("users")
        .delete()
        .match({username: UID})

        goto("/");
            
    }
    
    
        </script>