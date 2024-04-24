<BottomBar />

<button class="btn btn-solid-primary" on:click={() => {        
    findMentor = false; 
    becomeMentorBool = true;
    listSkills();}}>Become a Mentor</button>
<br>
<button class="btn btn-solid-secondary" on:click={() => {        
    findMentor = true; 
    becomeMentorBool = false;
    listSkills();}}>Search for a Mentor</button>

{#if becomeMentorBool}
<div class="mentorship-container">
    <p class="bmHeading">Become a mentor</p>
    <p class="text1">Pick up to five skills that you excel in and would like to help a mentee with.</p>
    <div class="controls">
        <select style="width:200px; display:inline-block;" bind:value={currentlyselectedskill}>
            {#each allskills as s}
                <option value="{s.skilltype}">{s.skilltype}</option>
            {/each}
        </select>
        <button class="btn btn-primary" on:click={addSkill}>Add</button>
        <button class="btn {editMode ? 'btn-warning' : 'btn-secondary'}" on:click={toggleEditMode}>
            {#if editMode}Done{:else}Edit{/if}
        </button>
    </div>
    <div class="skill-set">
        {#each currentSkills as a}
    <button 
        class="skill-button {selectedSkillsForRemoval.has(a.currentskill) ? 'selected' : ''}"
        on:click={() => toggleSkillSelection(a.currentskill)}>
        {a.currentskill}
    </button>
{/each}
    </div>
    <div class="action-buttons">
        {#if editMode}
            <button class="btn btn-danger" on:click={removeSelectedSkills}>Remove Selected</button>
        {/if}
        <button class="btn btn-primary" on:click={saveMentor}>Save</button>
    </div>
</div>
{/if}

{#if findMentor}
<div class="mentorship-container">
    <p class="bmHeading">Find a mentor</p>
    <p class="text1">Pick up to five skills that you would like a mentor to help you improve.</p>
    <div class="controls">
        <select style="width:200px; display:inline-block;" bind:value={currentlyselectedskill}>
            {#each allskills as s}
                <option value="{s.skilltype}">{s.skilltype}</option>
            {/each}
        </select>
        <button class="btn btn-primary" on:click={addSkill}>Add</button>
        <button class="btn {editMode ? 'btn-warning' : 'btn-secondary'}" on:click={toggleEditMode}>
            {#if editMode}Done{:else}Edit{/if}
        </button>
    </div>
    <div class="skill-set">
        {#each currentSkills as a}
            <button 
                class="skill-button {editMode && selectedSkillsForRemoval.has(a.currentskill) ? 'selected' : ''}"
                on:click={editMode ? () => toggleSkillSelection(a.currentskill) : null}
            >
                {a.currentskill}
            </button>
        {/each}
    </div>
    <div class="action-buttons">
        {#if editMode}
            <button class="btn btn-danger" on:click={removeSelectedSkills}>Remove Selected</button>
        {/if}
        <button class="btn btn-primary" on:click={findMentorFunc}>Search</button>
    </div>
</div>
    {#if searchPerformed}
        {#if eligibleMentors.length > 0}
            {#each eligibleMentors as i}
                <p>{i.first} may be a suitable mentor for you. Contact them via:</p>
                <p class="text4"><a href="mailto:{i.email}">{i.email}</a></p>
            {/each}
        {:else}
            <p>Sorry, there are no mentors that can help you with these skills at the moment. Please try again later.</p>
        {/if}
    {/if}

{/if}


<style>
    .mentorship-container {
    position:relative; 
    width:600px; 
    max-width:98%; 
    background-color:  #1F2833;
    margin: 0 auto;
    padding: 20px;
    box-sizing: border-box;
    }

    .bmHeading {
        font-weight: bold;
        font-size: 24px;
        color: white;
    }

    .text1 {
        color: white;
    }

    .text4 {
        color: blue;
        font-style: underline;
    }

    .controls, .skill-set, .action-buttons {
        margin-bottom: 15px;
    }

    .skill-button {
        padding: 6px 12px;
        margin-right: 10px;
        margin-bottom: 10px;
        border: 1px solid transparent;
        border-radius: 20px;
        background-color: #2C3E50;
        color: white;
        font-size: 14px;
        cursor: pointer;
        transition: background-color 0.3s, border 0.3s;
    }

    .skill-button.editable:hover {
        background-color: #34495E;
    }

    .skill-button.selected {
        background-color: #ECC94B; 
        border-color: #ECC94B;
    }

    .action-buttons {
        display: flex;
        justify-content: space-between;
        margin-top: 20px;
    }


</style>

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
    let searchPerformed = false;
    let editMode = false;
    let selectedSkillsForRemoval = new Set();


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
            location.reload();
    }

    let eligibleMentors = [];

    async function findMentorFunc()
    {
        searchPerformed = true;
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

            let collectUsernames = [];

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
                if(collectUsernames.includes(findSkills.data[i].username))
            {
                continue;
            }
            collectUsernames.push(findSkills.data[i].username)

            let collectFnames = [];

            for(let a = 0; a < arrayofskills.length; a++)
            {
                if(collectFnames.includes(findSkills.data[i].FirstName))
            {
                continue;
            }
            collectFnames.push(findSkills.data[i].FirstName)

                if(selectskills.includes(arrayofskills[a]))
                {
                    eligibleMentors = [
                        ...eligibleMentors,
                        {
                            "name" : findSkills.data[i].username,
                            "email" : findSkills.data[i].email,
                            "first" : findSkills.data[i].FirstName
                        }
                    ];
                } else
                {
                   console.log("false");
                }
            }
        }

    }  
        
    }

    function toggleEditMode() {
        editMode = !editMode;
        if (!editMode) {
            selectedSkillsForRemoval.clear();
        }
    }

    function toggleSkillSelection(skill) {
    if (selectedSkillsForRemoval.has(skill)) {
        selectedSkillsForRemoval.delete(skill);
    } else {
        selectedSkillsForRemoval.add(skill);
    }
    }

    function removeSelectedSkills() {
        currentSkills = currentSkills.filter(skill => !selectedSkillsForRemoval.has(skill.currentskill));
        selectedSkillsForRemoval.clear(); 
        editMode = false; 
    }

</script>