<BottomBar />

<button on:click={function(){        
    findMentor = false; 
    becomeMentorBool = true;
    listSkills();}}>Become a Mentor</button>
<br>
<button on:click={function(){        
    findMentor = true; 
    becomeMentorBool = false;
    listSkills();}}>Search for a Mentor</button>

{#if becomeMentorBool}
<div 
style="position:relative; height:400px; width:600px; max-width:98%; 
background-color: lightgrey; left:50%; transform:translateX(-50%);
padding: 20px 20px 20px 20px">

<p>Become a mentor</p>
<br><br>
<select style="width:200px" bind:value={currentlyselectedskill}>
{#each allskills as s}
<option value="{s.skilltype}">{s.skilltype}</option>
{/each}
</select>

<button on:click={addSkill}>Add</button>
<br><br>
{#each currentSkills as a}
<span class="badge badge-primary">{a.currentskill}</span>
{/each}
<br><br>
<button on:click={saveMentor}>Save</button>
</div>
{/if}

{#if findMentor}
<div 
style="position:relative; height:400px; width:600px; max-width:98%; 
background-color: lightgrey; left:50%; transform:translateX(-50%);
padding: 20px 20px 20px 20px">

<p>Find a mentor</p>
<br><br>
<select style="width:200px" bind:value={currentlyselectedskill}>
{#each allskills as s}
<option value="{s.skilltype}">{s.skilltype}</option>
{/each}
</select>

<button on:click={addSkill}>Add</button>
<br><br>
{#each currentSkills as a}
<span class="badge badge-primary">{a.currentskill}</span>
{/each}
<br><br>
<button on:click={findMentorFunc}>Search</button>
</div>
{/if}

{#each eligibleMentors as i}
<p>{i.name}</p>
<p><a href="mailto:{i.email}">{i.email}</a></p>
{/each}

<script>

    import BottomBar from "$lib/BottomBar.svelte"
    import { onMount } from "svelte";
    import { goto } from "$app/navigation";
    import {createClient} from "@supabase/supabase-js";

    const supabase = createClient("https://tkkcqbnwdharnagmhten.supabase.co", "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InRra2NxYm53ZGhhcm5hZ21odGVuIiwicm9sZSI6ImFub24iLCJpYXQiOjE3MTEzOTk2MDMsImV4cCI6MjAyNjk3NTYwM30.VyF3VVorlbPus_GBz8YXK4c-xQ3jInSyxiNSTndDTpg")
  
    let fname;
    let username;
    let userID;

    let becomeMentorBool = false;
    let findMentor = false;
    let currentlyselectedskill = "";

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
               
                let getFirstName = await supabase
                .from("users")
                .select()
                .match({email: localStorage.getItem("email")})

                fname = getFirstName.data[0].FirstName;
                username = getFirstName.data[0].username;
                userID = getFirstName.data[0].id;

            } else
            {
                goto("/")
            }
        } else
        {
            
            goto("/")
        }
    }

    let allskills = [];
    async function listSkills()
    {
        allskills = [];
        currentSkills = [];

        let getskills = await supabase
        .from("skills")
        .select()

        for(let i = 0; i < getskills.data.length; i++)
        {
            allskills =
            [ ...allskills,
                {
                'skilltype': getskills.data[i].skill
                }
            ]
        }

    }

    let currentSkills = [];
    function addSkill()
    {
        if(currentSkills.length == 5)
        {
            return;
        }

        if(currentSkills.some(item => item.currentskill === currentlyselectedskill))
        {

        } else
        {
            currentSkills = 
        [
            ...currentSkills,
            {
                "currentskill" : currentlyselectedskill
            }];
     
        }

    }

    async function saveMentor()
    {
        if(currentSkills.length > 0)
        {
            let makeMentor = await supabase
            .from("users")
            .update({isMentor: true})
            .match({username: username})
        }

        let listofskills = "";
        for(let i = 0;  i < currentSkills.length; i++)
        {
           listofskills += currentSkills[i].currentskill + ", ";
        }

        let addSkills = await supabase
            .from("users")
            .update({skills: listofskills})
            .match({username: username})
    }

    let eligibleMentors = [];

    async function findMentorFunc()
    {
        let findSkills = await supabase
            .from("users")
            .select()
            .match({isMentor: true})

            let arrayofskills = [];
            let userID = [];
            eligibleMentors = [];

            let selectskills = [];
            for(let i = 0;  i < currentSkills.length; i++)
            {
            selectskills.push(currentSkills[i].currentskill)
            }

        for(let i = 0; i < findSkills.data.length; i++)
        {
            arrayofskills = [];

            arrayofskills = findSkills.data[i].skills.split(",")
            for(let x = 0; x < arrayofskills.length; x++)
            {
                arrayofskills[x] = arrayofskills[x].trim();
            }

            for(let a = 0; a < arrayofskills.length; a++)
            {
                if(selectskills.includes(arrayofskills[a]))
                {
                    eligibleMentors = [
                        ...eligibleMentors,
                        {
                            "name" : findSkills.data[i].username,
                            "email" : findSkills.data[i].email
                        }
                    ];
                } else
                {
                    console.log("false")
                }
            }
        }

        
        
    }
</script>