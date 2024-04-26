<div class="background">
    <br>
<div class="page-container">
    <h1>General Health and Fitness Guide</h1>
    <button on:click={nextTip}>Next Tip</button>
    <div>
        <h2>{healthTips[currentIndex].title}</h2>
        <ul>
            {#each healthTips[currentIndex].content as tip}
                <li>{tip}</li>
            {/each}
        </ul>
    </div>
</div>
</div>

<style>
    .background {
        background: rgb(63,94,251);
        background: linear-gradient(167deg, rgba(63,94,251,1) 0%, rgba(252,70,107,1) 100%);
        height: 700px;
    }
    .page-container {
        max-width: 95%;
        margin: 40px auto;
        padding: 40px;
        background-color: #f4f4f4; 
        border-radius: 10px;
        box-shadow: 0 10px 20px rgba(0,0,0,0.1);
        font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif;
    }

    button {
        display: block;
        margin: 20px auto;
        padding: 10px 20px;
        background-color: #0056b3;
        color: white;
        border: none;
        border-radius: 5px;
        cursor: pointer;
    }

    button:hover {
        background-color: #003580;
    }

    h1 {
        color: #0056b3;
        text-align: center;
        font-size: 2.5em;
        font-weight: bold;
        margin-bottom: 30px; 
    }

    h2 {
        color: #0056b3;
        margin-top: 30px;
        font-size: 1.5em;
        font-weight: bold;
        margin-bottom: 15px;
    }

    ul {
        list-style: none;
        padding: 0;
    }

    li {
        background-color: #ffffff;
        margin-bottom: 15px;
        padding: 15px;
        border-left: 4px solid #0056b3; 
        font-size: 1em; 
    }

    .sub-heading {
        font-weight: bold;
        margin-bottom: 5px;
        font-size: 1.2em; 
    }

    @media (max-width: 768px) {
        .page-container {
            padding: 20px;
        }

        h1 {
            font-size: 2em;
        }

        h2 {
            font-size: 1.3em;
        }
    }
</style>


<BottomBar />

<script>
// Initialize the index for the current health tip being displayed
let currentIndex = 0;

// Array containing objects with health tips and their corresponding content
    const healthTips = [
        {
            title: "Start Slow",
            content: [
                "Assess your current fitness level and start exercising at a pace that feels challenging but not overwhelming.",
                "Begin with low-impact exercises such as walking, swimming, or cycling. Gradually increase the intensity and duration of your workouts as your fitness improves."
            ]
        },
        {
            title: "Establish a Routine",
            content: [
                "Try to establish a new routine that includes specific times for physical activity. Consistency will help make exercise a regular part of your life.",
                "Setting achievable fitness goals can provide a sense of purpose and accomplishment."
            ]
        },
        {
            title: "Seek Support",
            content: [
                "Many communities in the UK offer support programs for ex-offenders, including fitness and wellness programs.",
                "Work with a fitness professional who can design a program tailored to your needs and limitations."
            ]
        },
        {
            title: "Focus on Nutrition",
            content: [
                "Focus on a balanced diet rich in fruits, vegetables, lean proteins, and whole grains.",
                "Drinking plenty of water is essential, especially as you increase your physical activity."
            ]
        },
        {
            title: "Overcoming Mental Barriers",
            content: [
                "Engage in activities like yoga and meditation to improve both mental and physical health.",
                "Celebrate small victories and stay positive, even when progress seems slow."
            ]
        }
    ];
    
    // Function to advance to the next health tip in the array, looping back to the start if at the end
    function nextTip() {
        currentIndex = (currentIndex + 1) % healthTips.length;
    }

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
    </script>