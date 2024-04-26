<div class="container">
    <section>
      <h1>About Us</h1>
      <p>Welcome to RestartHub, your digital ally in navigating the journey back into society after incarceration.</p>
    </section>
  
    <section in:fade={{ delay: 200, duration: 800 }}>
      <h2>Our Mission</h2>
      <p>At RestartHub, our mission is to dismantle the barriers to reintegration with our cutting-edge, user-friendly platform.</p>
    </section>
  
    <section class="fade-in-section" bind:this={showChallenge} on:mouseenter={() => showChallenge = true}>
      {#if showChallenge}
        <div transition:scale={{ delay: 200, duration: 400 }}>
          <h2>The Challenge</h2>
          <p>Reintegration is not a one-size-fits-all process. Ex-offenders must grapple with finding suitable employment, securing housing, and rebuilding community ties.</p>
        </div>
      {/if}
    </section>
  
    <section class="fade-in-section" bind:this={showSolution} on:mouseenter={() => showSolution = true}>
      {#if showSolution}
        <div transition:scale={{ delay: 200, duration: 400 }}>
          <h2>Our Solution</h2>
          <p>Leveraging the latest in technology, RestartHub simplifies the journey post-incarceration.</p>
          <ul>
            <li>Intelligent job-matching algorithms to connect you with roles that match your skills and background.</li>
            <li>Community forums and mentorship opportunities for peer support and growth.</li>
            <li>A user-friendly interface accessible across multiple devices.</li>
          </ul>
        </div>
      {/if}
    </section>
  
    <section>
      <a href= '/dashboard'><button>Explore</button></a>
    </section>
  </div>

  <style>

  
    .container {
      width: 10000px;
      max-width: 100%;
      margin: auto;
      padding: 4rem 1rem;
      text-align: center;
      margin: 0;
      font-family: 'Open Sans', sans-serif;
      background: rgb(63,94,251);
      background: linear-gradient(167deg, rgba(63,94,251,1) 0%, rgba(252,70,107,1) 100%);
      color: white;
      line-height: 1.6;
    }
  
    h1, h2 {
      color: turquoise; 
      font-weight: bold;
      font-size: 20px;
    }
  
    p, li {
      margin-bottom: 1rem;
    }
  
    button {
      background-color: #66FCF1;
      color: #0B0C10;
      padding: 0.75rem 1.5rem;
      border: none;
      border-radius: 4px;
      cursor: pointer;
      font-size: 1rem;
      transition: transform 0.2s ease;
    }
  
    button:hover {
      transform: scale(1.05);
    }
  
    section {
      margin-bottom: 2rem;
    }
  
    .fade-in-section {
      display: grid;
      place-items: center;
      min-height: 20vh;
    }
  </style>

<BottomBar />
<script>
    import BottomBar from "$lib/BottomBar.svelte"
    import { onMount } from "svelte";
    import { goto } from "$app/navigation";
    import {createClient} from "@supabase/supabase-js";
    import { fade, scale } from 'svelte/transition';

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

    // Triggers the animations when the content is in view.
    let showMission = false;
    let showChallenge = false;
    let showSolution = false;
</script>